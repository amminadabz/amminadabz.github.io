# How to approach a just Stradella keyboard

## Foreword

This article is a written record of my thought process in conceptualizing the tuning algorithm. As such it should be an effective, if not the most efficient, means of understanding why the algorithm works the way it does: each problem that it needs to address, and how it addresses it.

## Tuning array

Now we must begin stacking and nesting all of the mathematical systems in harmony and intonation. 

First, a global tonal center must be established, ideally for the keynote of the song. A possible default is the common A = 440 Hz. We will call this pitch **GTC**. GTC will be placed at the root of the home row (in red) 

From this starting point, the circle of fifths can be filled out. Moving clockwise, multiply by 3:2, and moving counterclockwise, by the inverse 2:3. You now have a one dimensional array which extends infinitely along the horizontal axis, but your keyboard does not. This is because *a just tempered circle of fifths does not loop back on itself*; repeated multiplication of 3:2 does not allow it. So it's really more of a spiral of fifths. 

Next, one must build out a scale for each column of this array, creating the second dimension. It would be optimal to include in this dimension alternative ratios for some intervals, such as the 9:5 minor seventh or the 7:6 blues minor third.  

This leaves you with a two dimensional array with infinite root columns and more than 12 (maybe 14) scale degree rows.  Like we said, the keyboard cant be infinite, so we have to choose the breadth of the harmonic ambitus that one can reasonable be expected to need in one song. For now, we are working with the tried and true 12 root layout (Db to F#) based on a 48 bass (12x4) accordion. So that gives us a 12 by 14ish array. 

This can be represented and automatically generated in a spreadsheet, and in fact, it is. See documentation/tuning_array.ods 

## Lets talk about chord voicings

There are a couple of approaches we could take to chord voicings. One is to decide on a few preset voicings and let the user toggle between them. Another is to provide a sort of slider to adjust how many high and low notes to add. And yet another is to apply a sort of algorithm to act as a high and low pass filter (though not actually a filter, just a control on the volume of certain notes in the chord) so that the chord fades out into the high and low end, again with either a toggle or an analog adjustment. This gives the effect of every chord sounding like a segment of a Sheppard tone, so every chord sounds like it is occupying the same area of pitch space.

## Interaction with a piano keyboard

It is important to consider whether the just tuning method designed for a stradella keyboard will be compatible with a piano keyboard, and even with itself. 

The strategy of laying out the tuning for every note from the one GTC breaks down as soon as one moves an interval other than a perfect fifth. If, for example, one wanted to move from a C major chord to a D minor chord: that d would be in tune with g, which would be in tune with c, but there is no transitive property of just intonation to say that the d is in tune with the C, and in fact the act of repeatedly stacking fifthes goes aginst the spirit of just intonation. Such a method is called Pythagorean tuning, and its benefits and detriments are a topic for another day.

So then, it seems the most logical approach is to generate tuning tables on the fly.

## Examples

Let's walk through some examples and explore the options for a tuning approach. We'll start with a simple functional progression:

![](progression1.png)

One of the most basic progressions in functional harmony, `I IV V I`. Using a semi-Pythagorean approach guarantees that we end up back where we started, avoiding comma pumps and keeping GTC (C=261) constant. In this approach, the roots of each chord are calculated ahead of time by stacking just fifths, and the other notes of the chord are derived using their own interval ratios from their roots. Each chord is perfectly in tune with itself, but not necessarily with the chords around it.

![](progression1_semi-pythagorean-1.png)

A problem arises when we try to add a melody to this progression.

![*An unpleasant melody to prove a point.*](progression1_melody2.png)

What should this melody be tuned in relation to, and why? Again, there are options we must choose from. We could consider the melody its own separate entity which just happens to share GTC with the progression. This ignores the role the passing notes play as extensions of the chords below them. Many of the notes are in tune with the chords anyway; all of the notes over C chords naturally share the same tuning array, and the close functional relationships of F and G chords to C provide significant overlap in their tuning arrays. But our melody purposefully avoids those consonant notes to show how many functional extensions are out of tune.

![](progression1_melody2-2.png)

To clarify my methodology in calculating these offsets, here's an excerpt from [the spreadsheet](semi-pythagorean_tuning_array.ods) I used to compute scale degrees for each root. (remember to double or halve for octave correction) I can't be bothered to convert things into cents, so to put these frequency differences in perspective, the B flat in measure two is 7.25 Hz away from being in tune with an F chord, hanging between its fourth and its tritone. The difference between the fourth and the tritone is 23.2 Hz, so our percent error is 31.25%. As expressed by the insufferable pedantry throughout the paper up to this point, that is unacceptable.

| **degrees\\roots** | F       | C       | G       |
| ------------------ | ------- | ------- | ------- |
| 1                  | 174     | 261     | 195.75  |
| 1#                 | 185.6   | 278.4   | 208.8   |
| 2                  | 195.75  | 293.625 | 220.219 |
| 3b                 | 208.8   | 313.2   | 234.9   |
| 3                  | 217.5   | 326.25  | 244.688 |
| 4                  | **232** | 348     | 261     |
| 4#                 | 243.6   | 365.4   | 274.05  |
| 5                  | 261     | 391.5   | 293.625 |
| 6b                 | 278.4   | 417.6   | 313.2   |
| 6                  | 290     | 435     | 326.25  |
| 7b                 | 304.5  |**456.75**| 342.563 |
| 7b alt             | 313.2   | 469.8   | 352.35  |
| 7                  | 326.25  | 489.375 | 367.031 |

So lets tweak our technique and address the glaring problem: retune the melody to fit each underlying chord. Seems simple enough, until I come up with another convoluted melody to break the system >:)

![*Bendetti who?*](progression1_melody3.png)

Holding melody notes over chord changes exposes the new problem that comes with the new solution. If the melody is tuned tuned to the chords, what happens to a held melody note when the chord changes? One option is to quickly portamento these held notes into their new tunings and hope the listener doesn't notice. A more subtle option is to offset the tuning of the new chord, backsolving the frequency of the held note into the ratio for the new scale, but this goes against the main principle of our semi-Pythagorean approach; the roots of chords are no longer fixed, GTC drifts all over the place, the world descends into chaos, etc. At this point we must question whether having a comma-pump-proof system is really worth it. 

Man, this is exhausting.

Alright, lets get our priorities straight. Relationships between notes sounding at the same time matter more than notes played one after the other. But melodies can't (read: generally don't) hold over bar lines forever. Everyonce in a while, there will be a rest, or a block jump, and I'm willing to bet that relationships between consecutive notes matter so little that we can use these breaks to offset GTC back towards where it started.

![*Oooh, an A minor, its getting spicey!*](progression1_melody3-2.png)

So now lets apply everything we've learned so far.

![*A bit of notation: '4@C' means the fourth degree of a just C scale. We're also giving '=' some directionality here.*](progression1_melody3-3.png)

I imagine there's a limit to how big of a tuning jump one can make without the average listener noticing. This will have to be determined, as well as the length of time after which the size of the jump becomes irrelevant; after probably 30 seconds or so, who would remember that C used to be 20 cents flatter? It should also be considered whether two notes of the same intended pitch played in rapid succession should be treated by the tuner as one continuous note; the effect of this would be most noticeable in neo-Riemannian motions:

![](progression2a.png) **-VS-** ![](progression2b.png)

Perhaps the relationships between consecutive notes cannot be *entirely* ignored.

## What about non-tertian harmony?

Idk man.

So the priority should be as follows: 

- **Case 1:** if there are no held notes and no immediately repeated notes change GTC to be a set amount closer to its starting position. If there is a chord or manual root selection, tune all notes relative to that.

- **Case 2:** if a note is held while a chord change takes place, the new chord should be tuned relative to that note and GTC offset to match.

- **Case 3:** if no notes are held, and a chord changes immediately, the root of the new chord should be derived as a scale degree of the root of the previous chord, and everything else tuned in relation to that new root.

- **(Optional) Case 4:** If the user toggles this case on, then when a chord change takes place and the new chord shares one or more note(s) with the previous chord, those notes will maintain the same frequency while the rest of the new chord is derived from their pitches. If their maintained pitch is incompatible with their new function in the chord, the lowest common notes are to be prioritized in keeping their frequencies while higher notes may be adjusted as needed. 

And a few sub-cases:

- When a dominant seventh chord is played, either the third or seventh will be retuned to their alternate, depending on user preference and priority inherited from the held-note cases.
