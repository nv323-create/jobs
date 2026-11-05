# CLAUDE.md

@LESSONS.md

## About this project
- This is a course research project.
- The data is CPS ASEC from IPUMS.
- The analysis lives in a Python notebook (.ipynb).
- The notebook runs in Google Colab.

## Project
- Question: How much has the gender wage gap closed in the US since 1976, and for whom?
- Data file: `jobs_assignment_data_20261104.csv`, in the root of My Drive. In Colab the path is `/content/drive/My Drive/jobs_assignment_data_20261104.csv`. The file is about 1.6 GB, so read only the columns we need.
- Data content: CPS ASEC 1976-2025 from IPUMS. Ages 16-64, worked last year, positive wages. Weight: `asecwt`. The `year` column is the income year (the year the wages were earned).
- Main sample: full-time workers. Full-time means 35 or more hours a week (the IPUMS definition). The flag is `fulltime`. The flag has no weeks rule.
- Hourly wage: in 2025 dollars. The `_real` wage columns are already in 2025 dollars. Trim the bottom 1% and top 1% within each year, with men and women together (one cutoff per year, not one per sex).
- Gap: women's hourly wage as a % of men's. Also report men minus women in dollars.
- Report both the median and the mean wage, weighted with `asecwt`.

### Planned tables
All tables are by year.
1. Median and mean hourly wage by sex, with the gap.
2. Gap for full-time workers vs all workers.
3. Gap by age group.
4. Gap by education.
5. Gap by race and ethnicity.
6. Gap by marital status.
7. Gap by number of children.
8. Gap by child under 5.
9. Gap by occupation.
10. Gap by class of worker.
11. Gap by metro vs non-metro.

### Columns in the data
The file has 85 columns and 3,763,273 rows. "Clear" means the meaning is settled. "Check" means something is still unknown, so do not rely on it until it is fixed. Codes were printed in Colab on 2026-11-05.

| Column | What it means | Status |
|---|---|---|
| `year` | Income year, 1976 to 2025. | Clear |
| `period`, `period_label` | 1 = 1970s, 2 = 1980s, 3 = 1990s, 4 = 2000s, 5 = 2010s, 6 = 2020s. The data starts in 1976, so the 1970s group is 1976 to 1979 only. | Clear |
| `asecwt` | Person-level survey weight. Use it in every table and chart. | Clear |
| `age` | Age in years. | Clear |
| `agegrp`, `agegrp_label` | 1 = 16-24, 2 = 25-34, 3 = 35-44, 4 = 45-54, 5 = 55-64. | Clear |
| `female`, `female_label` | 0 = Male, 1 = Female. No missing values. | Clear |
| `fulltime`, `fulltime_label` | 1 = full-time, 0 = not. In the data, `hrswk` is 35 to 99 when it is 1, and 1 to 34 when it is 0. Weeks worked play no part. | Clear |
| `wkswork` | Weeks worked last year, 1 to 52. | Clear |
| `hrswk` | Hours worked per week, 1 to 99. The 99 may be a cap. Not known if these are usual or actual hours. | Check |
| `incwage_real` | Yearly wage and salary income, in 2025 dollars. | Clear |
| `wkearn_real` | Weekly earnings, in 2025 dollars. Not known how it was built. | Check |
| `hrwage_real` | Hourly wage, in 2025 dollars. No missing values. It runs from 0.0006 to 603,445 dollars an hour, so it must be trimmed. Not known how it was built. | Check |
| `educ5`, `educ5_label` | Education, 5 groups. The codes have not been printed yet. | Check |
| `raceeth`, `raceeth_label` | 1 = Hispanic, 2 = White, 3 = Black, 4 = Other. Some rows are missing. The four flags below never overlap, so White and Black most likely mean non-Hispanic. | Check |
| `hispanic`, `white`, `black`, `other_race` | 0/1 flags that never overlap. 26,041 rows have none of the four set, probably the rows where `raceeth` is missing (not checked). Use `raceeth` instead. | Clear |
| `marst3`, `marst3_label` | Marital status, 3 groups. The codes have not been printed yet. | Check |
| `nchild_cat`, `nchild_cat_label` | 0, 1, 2, or 3+ children. | Clear |
| `child_u5`, `child_u5_label` | Child under 5, coded 1 or 0. The labels have not been printed yet. | Check |
| `occ6`, `occ6_label` | Occupation, 6 groups. The codes have not been printed yet. | Check |
| `classwkr3`, `classwkr3_label` | 1 = Private, 2 = Government. The name says 3 groups but the file has only these 2, and none are missing. The third group (probably self-employed) is not in the file. | Check |
| `region4`, `region4_label` | Census region, 4 groups. Not in the planned tables, so do not load it. | Clear |
| `metro`, `metro_label` | 1 = In a metro area, 0 = Not in a metro area. Some rows are missing. | Check |
| `*_label` columns | Text label for the code in the column with the same stem. | Clear |
| `female_0`, `female_1`, `fulltime_0`, `fulltime_1`, and every other `name_number` column | 0/1 dummy columns, one per code. They repeat what the code column already says. Do not load them. | Clear |

Load only the columns we need (use `usecols`), because the file is 1.6 GB.

### Settled
- Year: `year` is the income year.
- Dollars: all `_real` wage columns are in 2025 dollars. No CPI adjustment needed.
- Full-time: 35 or more hours a week (IPUMS). There is no full-year rule, so a person who worked 1 week at 40 hours counts as full-time.

### Open questions
- Trimming: cut the bottom and top 1% within each year. For the "all workers" table, do we take the cutoffs from all workers or from full-time workers only? Are the cutoffs weighted with `asecwt`? Waiting on the user.
- Class of worker: why does `classwkr3` have only Private and Government? Were the self-employed removed on purpose? Waiting on the user.
- Hourly wage: find out how `hrwage_real` and `wkearn_real` were built. Test it against `incwage_real` divided by weeks and hours.
- Missing values: count the missing rows in `raceeth` and `metro` (and in every grouping column) before the group tables.
- Top codes: check `hrswk` at 99, and the extremes of `hrwage_real`, before any math.
- Print the codes for `educ5`, `marst3`, `occ6`, and `child_u5`.

## Code rules
- Write simple code. Pick the plain way over the clever way.
- Put a short comment on each line of code.
- Use pandas, numpy, and matplotlib unless there's a good reason to add more.
- Use clear variable names, like `income_2022` and not `x1`.
- Keep each notebook cell small and focused on one step.

## Colab rules
- Write code that runs top to bottom in a fresh Colab session.
- Put all `pip install` and import lines in the first cell.
- Load data from a path at the top of the notebook, so it's easy to change.
- Don't use local file paths. Use Google Drive or an uploaded file.

## Data rules
- Never edit the raw IPUMS file. Make changes on a copy.
- Use the IPUMS codebook to check what each variable means.
- Use asecwt for person-level survey weights. Use it in every table and chart.
- Weight results with asecwt when you report totals or averages.
- Check for missing and top-coded values (like 99999999) before any math.
- Don't commit large data files or anything with restricted data terms. IPUMS data can't be redistributed.

## Chart rules
- Before any chart, print the weighted table it will show, with labels and 1 decimal.
- A chart shows exactly the printed table, and axis labels give units.
- All y-axis scales and labels should include 0.

## How to explain changes
- Explain every change in plain English.
- Say what you changed, why you changed it, and what it does to the results.
- Show the logic step by step, like you're explaining it to a friend.
- If you're not sure about something, say so. Don't guess.

## Git rules
- Work on the designated feature branch.
- Always sync to main so you have the latest file in the repo.
- Commit with a clear message that says what changed.
- When the work is done, push the branch and merge it into main.
- Don't open pull requests.

## Lessons rules
- When I correct you or state a preference, add a line to LESSONS.md: what happened and the rule for next time.
- When I paste an error from Colab, or you find a mistake in your code, fix it and add a line on the cause.
- Update an existing line instead of repeating it. Keep each line short.
- Commit LESSONS.md with your other changes.
