# Ice Games — Video Game Sales Analysis (2012–2016)

Exploratory analysis of global video game sales data to identify market patterns
and support campaign planning for 2017.

---

## Context

This project was developed for Ice, an online video game retailer operating globally.
The dataset contains historical sales records up to 2016. The analytical scenario
assumes December 2016 as the reference point — the goal is to identify which
platforms, genres, and titles are best positioned for a 2017 campaign.

---

## Dataset

**Source:** `games.csv` — public domain dataset  
**Records:** 16,715 entries (raw) → 2,855 after methodological filtering  
**Period:** 1980–2016 (analysis focused on 2012–2016)

| Column | Description |
|---|---|
| `name` | Game title |
| `platform` | Platform (e.g. PS4, X360) |
| `year_of_release` | Release year |
| `genre` | Game genre |
| `na_sales` | North America sales (USD millions) |
| `eu_sales` | Europe sales (USD millions) |
| `jp_sales` | Japan sales (USD millions) |
| `other_sales` | Other regions sales (USD millions) |
| `critic_score` | Critic score (0–100) |
| `user_score` | User score (0–10) |
| `rating` | ESRB age rating |

---

## Methodology

### Preprocessing

- Standardised column names to snake_case
- Converted `year_of_release` to `int` and `user_score` to `float64`
  (original contained `'tbd'` string entries)
- Null values treated with a two-level hierarchical imputation function:
  1. Same title on other platforms → median of known values
  2. Same platform + genre → median as fallback
- `rating` nulls filled using mode (categorical variable — median not applicable)
- 2 records with no `name` or `genre` retained with `'unknown'` label
  (valid sales data, platform GEN, 1993)
- `total_sales` engineered as sum of all four regional sales columns

### Methodological Filters

Two sequential filters applied before the regional analysis:

1. **Active platforms:** only platforms with releases recorded in 2014–2016
   (10 platforms retained out of 31)
2. **Title window:** only titles released between 2012 and 2016

**Active platforms:** 3DS, PC, PS3, PS4, PSP, PSV, Wii, WiiU, X360, XOne

---

## Analysis

### Sales Distribution

- `total_sales` is highly right-skewed — a small number of titles account for most revenue
- The same pattern holds across all regional sales columns
- Median global sales per title: 0.17M USD

### Platform Lifecycle

- Console lifecycles range from 3 to 11 years in the dataset
- PS2, X360, PS3 and Wii show 10–11 year windows — consistent with the industry's
  roughly one-generation-per-decade pattern
- PS4 and XOne entered the dataset in 2013 and show consistent growth through 2016,
  while PS3 and X360 are in sharp decline over the same period

### Regional Profiles (2012–2016 window)

| Region | Top Platforms | Top Genres |
|---|---|---|
| North America | X360, PS4, PS3, XOne | Action, Shooter, Sports |
| Europe | PS4, PS3, X360, XOne | Action, Shooter, Sports |
| Japan | 3DS, PS3, PSV, PS4 | Role-Playing, Action |
| Other | PS4, PS3, X360, XOne | Action, Shooter, Sports |

NA, EU and Other share a convergent profile — unified content strategy applicable
across these three regions. Japan requires a separate approach: portables (3DS, PSV)
and Role-Playing dominate.

### Scores vs. Sales

- `critic_score` shows a moderate positive correlation with `total_sales` (r = 0.27)
- `user_score` shows no meaningful correlation with `total_sales` (r = −0.04)
- Moderate positive correlation between `critic_score` and `user_score` (r = 0.38)

### Hypothesis Tests

Both tests used **Welch's t-test** (`equal_var=False`, α = 0.05) applied to
`user_score` within the 2012–2016 filtered dataset.

`user_score` was selected as the test variable because it reflects the assessment
of the general public who actually purchased and played the titles — more directly
linked to future purchase decisions than `critic_score`, which tends to correlate
with production budget.

**Test 1 — PS4 vs XOne (user_score)**

| | PS4 | XOne |
|---|---|---|
| Mean user_score | 6.94 | 6.66 |
| Variance | 1.70 | 1.64 |

p-value = 0.008 → H₀ rejected. The difference in mean user scores between PS4
and XOne is statistically significant. PS4 titles receive higher user ratings
within the same console generation.

**Test 2 — Action vs Role-Playing (user_score)**

| | Action | Role-Playing |
|---|---|---|
| Mean user_score | 6.83 | 7.51 |

p-value < 0.001 → H₀ rejected. Role-Playing titles receive significantly higher
user scores than Action titles. Combined with regional data, this suggests the
Japanese preference for RPG is associated with higher perceived satisfaction —
not solely cultural. In NA and EU, Action leads in revenue despite lower scores,
indicating that market volume and user evaluation are independent dimensions.

---

## Key Findings

1. **PS4 is the primary platform for 2017** — growth in all regions, highest user
   satisfaction among current-generation consoles
2. **XOne is the secondary platform for NA and EU** — convergent genre profile
   with PS4, consistent growth since 2013
3. **3DS is the dominant platform in Japan** and should be treated separately
   from the global strategy
4. **Action and Shooter lead in NA, EU and Other** — unified content strategy
   applicable across these markets
5. **Role-Playing is the priority genre for Japan**, alongside portables
6. **Critic score is a weak auxiliary signal** — positive but moderate correlation
   with sales; not a reliable standalone predictor
7. **Platforms in decline** (PS3, X360) may sustain residual volume in 2017
   but do not justify priority investment

---

## Deliverables

- `notebook/analysis.ipynb` — full analysis pipeline (preprocessing → EDA → hypothesis tests → conclusions)
- `data/games_active.csv` — filtered dataset used for regional analysis and dashboard
- `outputs/tableau_link.txt` — Tableau executive dashboard

---

## Stack

Python · Pandas · NumPy · SciPy · Matplotlib · Seaborn · Tableau

---

## Author

Brian Rodrigues — [github.com/rodriguesbrian](https://github.com/rodriguesbrian) · [linkedin.com/in/rodrigues-brian](https://linkedin.com/in/rodrigues-brian)