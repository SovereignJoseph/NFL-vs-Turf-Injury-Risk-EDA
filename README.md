# Does Fake Turf Hurt NFL Players More Than Real Grass?

NFL players have been saying for years that fake turf wrecks their knees and ankles. Their union has asked the league to put real grass in every stadium. The league's own records have sometimes shown no real difference. I wanted to check it myself with data anyone can go get.

**Short answer: I found no proof that turf causes more leg injuries. I also found that my first answer was wrong, and I explain why below.**

## Research Question:

**Among NFL team-weeks, is the rate of lower-extremity injury-report entries higher for teams playing on turf than on grass, and is that difference moderated by temperature?**

## The data

Everything comes from the `nflreadpy` tool, which pulls free NFL data straight from the internet. I used the 2021 through 2025 regular seasons.

Two tables:

- **Game schedule.** One row per game. Tells me the field type, the roof type, and the temperature.
- **Injury reports.** One row per player who was listed as hurt that week.

## How I set it up

**One row = one team, one week.** Since a team plays one game a week, that means one row is also one game. I count how many players on that team's next injury list have a leg problem written next to their name.

**I match a game to the *next* week's list, not the same week's.** The injury list comes out before the game is played. So a player hurt in Sunday's game does not show up until the list after that. I also skip over bye weeks, so if a team has a week off, I look ahead to the next week they actually played.

**Leg injuries** means knee, ankle, hamstring, groin, calf, quad, thigh, hip, pelvis, glute, foot, toe, heel, Achilles, or shin. I search the injury text for those words.

**Missing temperatures.** Games in domes and under closed roofs have no temperature written down, because the building keeps the air the same all year. I filled those in with 72 degrees. Outdoor games with no temperature written down got dropped, because there is no honest way for me to guess what it was outside that day.

## The mistake I made, and how I caught it

My first version measured the **share** of the injury list that was leg injuries. That sounds fine, but it is broken. The bottom number changes for reasons that have nothing to do with legs. A team with 3 players listed who all have bad knees scores 100 percent. A team with 10 players listed and 5 bad knees scores 50 percent, even though that team is clearly worse off.

Worse, injury lists get longer as the season goes on. Teams listed about 8 players in week 1 and about 11 by week 17. The number of leg injuries stayed flat the whole time, near 6. So the share dropped from about 60 percent to about 51 percent just because of the calendar.

Here is the trap: warm games happen early in the season and cold games happen late. Temperature and week number move together almost perfectly in this data (a correlation of -0.65). So a share-based number will always show "more leg injuries when it is warm," even if nothing is actually happening. My first answer was the calendar dressed up as weather.

The fix was to count **leg injuries per team-week** instead. One team-week is one game, so the counting is already fair, and a week where nobody got hurt is a real zero instead of a blank.

## What I found

**Raw totals are about playing time, not danger.**

| Surface | Team-weeks | Leg injury entries | Per team-week |
| --- | --- | --- | --- |
| Grass | 1,174 | 7,191 | **6.13** |
| Turf | 1,064 | 6,388 | **6.00** |

Grass has a bigger total only because teams played on grass more often.

**Per game, the two surfaces look the same.** Turf came out 0.12 injuries *lower* than grass. The margin of error runs from -0.35 to +0.11, which easily covers zero. If anything, the number leans toward turf being fine.

**Temperature did nothing.** Across five temperature groups, leg injuries per team-week ranged only from 5.86 to 6.46, with no pattern going up or down. Indoor games came in at 5.87, right in the middle.

**Turf was not even worse in a steady way.** In the five temperature groups, the turf-minus-grass gap went +0.51, +0.12, +0.33, **-0.26**, +0.44. It flips sign. Every margin of error crosses zero.

**No single injury type stood out.** Per team-week, grass vs. turf was 1.87 vs. 1.83 for knees, 1.42 vs. 1.46 for ankles, and 0.84 vs. 0.78 for hamstrings.

## What this does not prove

- **I am counting names on a list, not real injuries.** A player with a four-week ankle problem shows up four times. So this measures how crowded the injury list is, not how many new injuries happened. A surface that causes longer-lasting injuries would look the same as one that causes more of them.
- **Field type and roof type are tangled together.** Almost all indoor fields are turf and most outdoor fields are grass. So part of what I am comparing is indoor vs. outdoor, not just turf vs. grass.
- **I am guessing which game caused the injury.** I assume an injury on next week's list came from last week's game. Reasonable, but I cannot prove it without play-by-play injury records.
- **I find injuries by searching for words** in text typed by team staff, which is not the same as a doctor's records.
- **This shows a link at best, never a cause.** I am not controlling for which teams play on turf, how they practice, or how much they travel.

## My take

I do not have evidence that turf causes more leg injuries. But that is not the same as saying the players are wrong. This data says nothing about how bad an injury is, how it happened, or how many games a player misses. A surface that hurts worse when it does hurt would not show up in a count like mine. Players are describing how the ground feels under their feet, and my numbers cannot see that at all.

The real lesson for me was about the bottom number of a fraction. I had clean data, a careful matching step, and a sensible way to sort fields, and I still got the wrong answer at first because I never stopped to check that my main number measured what I thought it measured.

## Files

- `Project_1_WorkBook.ipynb` — the whole project: loading, cleaning, charts, and write-up
```
