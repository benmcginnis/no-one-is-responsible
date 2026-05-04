# Seminary Road Crash Analysis

Supporting data and methodology for *Seminary Road — An Honest Analysis*, published in [No One Is Responsible](https://nooneisresponsible.substack.com) on May 2026.

The post examines the City of Alexandria's 2022 Seminary Road Project Evaluation, extends the crash record three years beyond the city's analysis using Virginia Roads crash data, and compares the city's headline 41% crash reduction figure against a methodology that separates the COVID disruption period from the post-project recovery period.

---

## Files

### `Seminary_Road_Corridor_Crashes_With_Phase.csv`
The full set of 91 crashes used in this analysis, with phase assignments. Each row is one crash.

| Column | Description |
|---|---|
| `phase` | Analysis phase: Pre-project, Implementation, COVID disruption, Post-COVID |
| `CRASH_DT` | Crash date and time (RFC format: `Thu, 26 Jan 2017 05:00:00 GMT`) |
| `CRASH_YEAR` | Four-digit year as string |
| `CRASH_SEVERITY` | VDOT severity code: A = fatal/severe injury, B = non-severe injury, C = possible injury, O = property damage only |
| `K_PEOPLE` | Number of people killed |
| `A_PEOPLE` | Number of people with severe injuries |
| `B_PEOPLE` | Number of people with non-severe injuries |
| `C_PEOPLE` | Number of people with possible injuries |
| `PERSONS_INJURED` | Total persons injured |
| `PEDESTRIANS_KILLED` | Pedestrians killed |
| `PEDESTRIANS_INJURED` | Pedestrians injured |
| `VEH_COUNT` | Number of vehicles involved |
| `COLLISION_TYPE` | VDOT collision type description |
| `WEATHER_CONDITION` | VDOT weather condition at time of crash |
| `longitude` | WGS84 decimal degrees |
| `latitude` | WGS84 decimal degrees |

### `seminary_road_corridor.geojson`
The corridor polygon used to select crashes. A single polygon in WGS84 (EPSG:4326) representing the study corridor between N Howard Street and N Quaker Lane, including intersection bulb-outs at both ends. This geometry was derived by working backward from the city's published annual crash counts to identify their most likely inclusion area. It reproduces the city's published figures to within one crash across all reported years from 2017 onward.

---

## Methodology

### Data source
Virginia Roads statewide crash database, accessed April 8, 2026. Public data available at: https://www.virginiaroads.org/datasets/crashdata-basic-1/explore

### Corridor selection
The corridor polygon was reconstructed to reproduce the city's published crash counts as closely as possible. The city's evaluation (linked below) reports annual crash totals but does not publish the underlying data or corridor geometry. The reconstructed polygon matches the city's published figures to within one crash across 2017-2022. The single-crash discrepancy in 2019 is likely attributable to a minor geometry difference at one intersection.

### Phase boundaries
| Phase | Start | End | Crashes | Years |
|---|---|---|---|---|
| Pre-project | Jan 1, 2017 | Sep 30, 2019 | 34 | 2.74 |
| Implementation | Oct 1, 2019 | Dec 31, 2019 | 6 | 0.25 |
| COVID disruption | Jan 1, 2020 | Dec 31, 2021 | 13 | 2.00 |
| Post-COVID recovery | Jan 1, 2022 | Jun 19, 2025 | 38 | 3.46 |

The Virginia Roads data begins at 2017. The city's evaluation includes 2015 and 2016 in their trend table, but those years are not in the independently verified dataset and are not used in any comparison in this analysis.

### Boundary crash exclusions
Two post-COVID crashes — February 2023 and 2025 — fall just outside the western terminus boundary and are excluded to maintain consistency with the corridor geometry used to reproduce the city's published counts. Neither would have appeared in the city's analysis, which predates both. Including them would increase the post-COVID annualized rate from 10.97 to 11.55 crashes per year, reducing the apparent improvement from 11.5% to approximately 6.8%.

### Statistical test
Pre-project versus post-COVID recovery comparison uses a Poisson exact rate test (scipy.stats.poisson_means_test). Result: rate ratio 0.885, 95% CI [0.557, 1.406], p = 0.608. The confidence interval spans from a 44% reduction to a 41% increase. The difference is not statistically distinguishable from random variation at conventional thresholds.

### Pre-2017 data
The city's evaluation reports 2015 and 2016 crash counts. These are accepted as reported in the evaluation for trend context but are not used in any quantitative comparison in this analysis, as they cannot be independently verified against the Virginia Roads dataset used here.

---

## Key findings

- The city's 41% crash reduction figure compares the pre-project period to a post-implementation period that includes the COVID-19 disruption (2020-2021), when crashes fell citywide due to reduced traffic volumes.
- Comparing pre-project to the post-COVID recovery period (2022-June 2025) produces an 11.5% reduction, or 6.8% if two boundary crashes are included.
- A Poisson rate test finds no statistically significant difference between pre-project and post-COVID crash rates (p = 0.608).
- The 85th percentile operating speed was 34 mph before the project and 34 mph after, per the city's own evaluation (Table 2, Technical Attachment).
- The city's claim of zero fatal or severe injuries since project completion was accurate as of October 2022. The Virginia Roads data shows two severe-injury crashes in summer 2024.
- Bicycle ridership increased from approximately 4 to 16 cyclists per peak period.

---

## Sources

- [City of Alexandria Seminary Road Project Evaluation (October 2022)](https://www.alexandriava.gov/sites/default/files/2022-11/Seminary%20Road%20Project%20Evaluation.pdf)
- [Seminary Road Evaluation - Technical Attachment](https://www.alexandriava.gov/sites/default/files/2022-11/Seminary%20Road%20Evaluation%20-%20Attachment.pdf)
- [Virginia Roads crash data for Alexandria](https://www.virginiaroads.org/datasets/crashdata-basic-1/explore?location=38.834052%2C-77.066827%2C13)

---

## License

The crash data in this repository is derived from Virginia Roads public data. The corridor polygon and phase assignments are original work and are released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). You are free to use, share, and adapt this work with attribution.
