# Do public holidays reduce bike rentals in Seoul — and by enough to change the supply plan?
Do public holidays cut bike rentals in Seoul, and by enough to change the supply plan?
1. The question

Do public holidays have fewer bike rentals per day than non-holidays, by enough to justify a reduced bike-supply schedule on holidays?

The decision behind the question is practical. A bike-share operator moves bikes between stations and schedules staff every single day. If holidays are much quieter, running the normal schedule wastes money. If the drop is small, or only looks large because of how the comparison was built, cutting supply risks leaving riders without bikes.

2. The answer

CHOOSE: Yes / No. On a public holiday, Seoul sees about {{EST}} CHOOSE: fewer / more bike trips per day than on comparable non-holidays (95% confidence interval {{LO}} to {{HI}}), which is {{REL}} percent CHOOSE: below / above a normal day.

The number to remember: about {{EST}} trips per day.

A simple comparison of holidays against all other days gives {{NAIVE}} trips per day. The controlled figure above is the one to use for planning, because it compares each holiday with days of the same type and season. The rest of this report earns that answer.

3. The data

The data is the Seoul Bike Sharing Demand dataset from the UCI Machine Learning Repository, released under a Creative Commons Attribution 4.0 licence. It contains 8,760 rows covering {{START}} to {{END}}, which is one full year. One row is one hour of system-wide rentals in Seoul, together with weather measurements, the season, a flag for public holidays and a flag for whether the system was functioning.

There are several things wrong or limiting about it. First, {{DOWN_HOURS}} hours are marked as non-functioning, and they show zero rentals. That is downtime, not real zero demand, so leaving them in would make some days look artificially quiet. Second, there are only {{HOL_DAYS}} public-holiday days in the whole year, so any holiday estimate rests on a small number of days. Third, it covers one city and one year, so it cannot show whether the pattern is stable over time. Finally, the dataset does not document how rentals were counted, so I cannot check whether the measurement method changed. I found CHOOSE: no missing values / the following missing values: ___ in the columns I used.

4. The analysis

I did the analysis in the following order.

Step 1: change the unit from hours to days. Hours within the same day are strongly related: a rainy afternoon lowers every hour that follows. Treating 8,760 hours as independent observations would give confidence intervals that are far too narrow. I therefore summed rentals to one total per day, and the day became the unit of analysis.

Step 2: remove incomplete days. I kept only days on which the system worked all 24 hours. This removed {{DOWN_DAYS}} days and ensured that a day is never low simply because the system was switched off.

Step 3: measure the naive gap. I compared the {{HOL_DAYS}} holiday days (average {{HOL_MEAN}} trips) with the {{NON_DAYS}} other days (average {{NON_MEAN}} trips), using a Welch interval that does not assume equal variances. The naive difference is {{NAIVE}} trips per day (95% interval {{NAIVE_LO}} to {{NAIVE_HI}}), or {{NAIVE_REL}} percent of the non-holiday average.

Step 4: look for what else differs between the groups. A holiday effect is only believable if holidays and other days are otherwise alike. I checked day type (weekend versus weekday), season and temperature.

Step 5: compare like with like. I split days into groups by day type and season, measured the holiday gap inside each group, and averaged those gaps, weighting each group by how many holidays it contains. I used a bootstrap with 5,000 resamples for the confidence interval, because the groups are small.

Step 6: test whether the answer depends on my choices. I repeated the controlled estimate six ways, described in section 6.

5. Alternatives ruled out

Chance. The naive interval runs from {{NAIVE_LO}} to {{NAIVE_HI}} trips per day. CHOOSE: It excludes zero, so the gap is larger than its own sampling uncertainty. / It includes zero, so chance cannot be ruled out. The controlled interval, {{LO}} to {{HI}}, tells the same story after adjustment.

Confounding. The non-holiday group is a mixture of very different kinds of day. {{SHARE_WKND_NON}} percent of non-holiday days are weekend days, compared with {{SHARE_WKND_HOL}} percent of holidays. The average temperature on holidays was {{TEMP_HOL}} degrees Celsius against {{TEMP_NON}} on other days. Weekends have no commuter peaks, and warm days attract more riders, so comparing a holiday with the average non-holiday mixes the holiday effect with day type and weather. After controlling for both, the estimate moved from {{NAIVE}} to {{EST}} trips per day, a change of {{DIFF}}. In plain words: CHOOSE and finish this sentence from your own numbers, for example "the naive comparison understated the holiday drop because many non-holiday days are weekends that are already quiet" or "the gap barely moved, so day type and season were not what was driving it".

Artefact. I checked whether the way the data was collected could create the gap. Downtime was {{DOWN_DAYS}} days, and none of it CHOOSE: fell on / fell disproportionately on holidays, so removing incomplete days does not bias the comparison. Holidays are spread across CHOOSE: many months of the year / only these months: ___, so no single season carries the result. What I cannot check is how the rentals were counted or whether the counting method changed during the year, because the dataset does not say.

6. Sensitivity

A conclusion that depends on one arbitrary choice is not a conclusion. I recomputed the controlled estimate under six variations: the main version (day type by season); day type only, a coarser control; day type by temperature in three bands; day type by temperature in two bands, which moves the band boundaries; weekdays only, which changes the subset; and the main version with unusually high or low days removed.

Across all six, the estimate ranged from {{S_MIN}} to {{S_MAX}} trips per day. CHOOSE: The estimate is stable: every version points the same way and the intervals overlap, so the conclusion does not hinge on how I controlled. / The estimate drifts: it is largest when ___ and smallest when ___. The direction of the drift suggests that remaining bias, most likely from ___, runs towards ___, so the true effect is probably CHOOSE: somewhat larger / smaller than my main figure.

7. Limits
With only {{HOL_DAYS}} holiday days, estimates are noisy, and holidays are not all alike. A single day off may behave very differently from a long holiday week.
The data covers one city and one year, so it says nothing about whether the effect holds in other years.
Neighbouring days share weather, so days are not fully independent, and my intervals may still be somewhat optimistic.
Weather is only partly controlled. I used season and temperature, but not rain, wind or air quality.
This shows a difference in demand. It does not say what the best supply schedule is, because the cost of a missing bike against the cost of an idle bike is not in the data.
8. What would change it
More years of data. If several further years showed a much smaller or opposite holiday effect, I would treat this year's result as a one-off.
Holiday type. If the drop were concentrated in a few long holiday periods and absent on single-day holidays, the supply plan should differ by holiday type instead of using one rule.
Time of day. If hourly data showed the drop is entirely in commuting hours, the right response would be to cut supply only at those hours, not all day.
Measurement change. Evidence that the counting method or station network changed during the year would undermine the comparison, because the gap could reflect the system rather than rider behaviour.
Better weather control. If adding rain and wind moved the estimate substantially, my controlled figure would need revising in that direction.