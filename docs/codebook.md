# Codebook

The data in `data/` are the authors' own files. Maglio and Polman (2014) posted
their data on the Open Science Framework (<https://osf.io/7rajd/>), and these
copies came to us through earlier versions of this course. We have not
changed them. There is one Excel file per study, each with a single sheet named
`Sheet1` and one row per participant. Study 1 has no missing values.

This lab uses **Study 1** only. The other files are here so you can try
another study once you finish.

## Study 1: `S1_Subway.xlsx` (202 rows)

| Column | Type | Values | Meaning |
|---|---|---|---|
| `DIRECTION` | text | `EAST`, `WEST` | Which platform the participant was on at Bay Street station, i.e. which way they were traveling |
| `DISTANCE` | number | 1–7 scale (observed 1–6) | "How far away does the [name] station feel to you?" 1 = very close, 7 = very far |
| `STN_NUMBER` | number | 1–4 | Station code, west to east: 1 = Spadina, 2 = St. George, 3 = Bloor-Yonge, 4 = Sherbourne |
| `STN_NAME` | text | `SPAD`, `STG`, `B-Y`, `SHER` | Station abbreviation; same information as `STN_NUMBER` |

Where the stations are, relative to the participants at Bay Street:

```
   WEST  <------------------------------------------------>  EAST

   Spadina     St. George     [ Bay ]     Bloor-Yonge     Sherbourne
   (2 west)    (1 west)       you are     (1 east)        (2 east)
                              here
```

A westbound traveler is moving **toward** Spadina and St. George and **away
from** Bloor-Yonge and Sherbourne. An eastbound traveler is the reverse.

`read_excel()` reads `DIRECTION` and `STN_NAME` as text (`<chr>`) and
`STN_NUMBER` as a number (`<dbl>`). For the ANOVA, convert direction and
station to factors (Step 3 of the report). If you use `STN_NUMBER` as the
station variable instead, it **must** be a factor: as a number, `aov()` treats
it as a single straight-line predictor and you get 1 degree of freedom
instead of 3.

## The other studies

Column names only. See the paper for what each study did.

| File | Rows | Columns |
|---|---|---|
| `S2_Facing.xlsx` | 80 | `DIRECTION`, `FACING`, `DISTANCE` |
| `S3a_Shoppers.xlsx` | 100 | `DIRECTION`, `MINUTES` |
| `S3b_Starbucks.xlsx` | 86 | `DIRECTION`, `TARGET`, `MINUTES` |
| `S4_probability.xlsx` | 50 | `CONDITION`, `HOW_LIKELY`, `FAMILIARITY` |
| `S5_LA_Chicago.xlsx` | 45 | `CONDITION`, `CLOSENESS` |
