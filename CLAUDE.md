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
- Data content: CPS ASEC 1976-2025 from IPUMS. Ages 16-64, worked last year, positive wages. Weight: `asecwt`. The `year` column is the income year (the year the wages were earned). Self-employed people were removed on purpose (their income has other sources and can be negative), so the file has wage and salary workers only.
- Main sample: full-time workers. Full-time means 35 or more hours a week (the IPUMS definition). The flag is `fulltime`. The flag has no weeks rule.
- Hourly wage: in 2025 dollars. The `_real` wage columns are already in 2025 dollars. Trim the bottom 1% and top 1% within each year, with men and women together (one cutoff per year, not one per sex). The cutoffs come from all workers in that year (full-time and not), weighted with `asecwt`. Use the same cutoffs in every table, including the full-time sample.
- Gap: women's hourly wage as a % of men's. Also report men minus women in dollars.
- Report both the median and the mean wage, weighted with `asecwt`.

### Planned tables
All tables are by year. Tables 3 to 11 also show the unweighted number of women and men (n) behind each gap, so thin cells are easy to spot.
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
The file has 85 columns and 3,763,273 rows, with all 50 income years present (55,270 to 101,128 rows a year). "Clear" means the meaning is settled. "Check" means something is still unknown, so do not rely on it until it is fixed. Codes were printed in Colab on 2026-11-05.

| Column | What it means | Status |
|---|---|---|
| `year` | Income year, 1976 to 2025. | Clear |
| `period`, `period_label` | 1 = 1970s, 2 = 1980s, 3 = 1990s, 4 = 2000s, 5 = 2010s, 6 = 2020s. The data starts in 1976, so the 1970s group is 1976 to 1979 only. | Clear |
| `asecwt` | Person-level survey weight. Use it in every table and chart. | Clear |
| `age` | Age in years. | Clear |
| `agegrp`, `agegrp_label` | 1 = 16-24, 2 = 25-34, 3 = 35-44, 4 = 45-54, 5 = 55-64. | Clear |
| `female`, `female_label` | 0 = Male, 1 = Female. No missing values. | Clear |
| `fulltime`, `fulltime_label` | 1 = full-time, 0 = not. Full-time means 35 or more usual hours a week last year. In the data, `hrswk` is 35 to 99 when it is 1, and 1 to 34 when it is 0. Weeks worked play no part. | Clear |
| `wkswork` | Weeks worked last year, 1 to 52. | Clear |
| `hrswk` | Usual hours worked per week last year, 1 to 99. 4,398 rows (0.1%) are exactly 99, which may be a cap. | Clear |
| `incwage_real` | Yearly wage and salary income, in 2025 dollars. | Clear |
| `wkearn_real` | Weekly earnings, in 2025 dollars. It equals `incwage_real` divided by `wkswork`. Tested: no difference. | Clear |
| `hrwage_real` | Hourly wage, in 2025 dollars. It equals `incwage_real` divided by (`wkswork` times `hrswk`). Tested: no difference. No missing values. It runs from 0.0006 to 603,445 dollars an hour, so it must be trimmed. | Clear |
| `educ5`, `educ5_label` | 1 = None/preschool, 2 = Less than HS, 3 = High school, 4 = Some college, 5 = Bachelor's+. No missing values. | Clear |
| `raceeth`, `raceeth_label` | 1 = Hispanic, 2 = White, 3 = Black, 4 = Other. White and Black are non-Hispanic (user confirmed). 26,041 rows (0.7%) are missing: the "don't know" and "no response" answers. By year it is at most 2.5% (1978) and 0.0% from 2003 on. | Clear |
| `hispanic`, `white`, `black`, `other_race` | 0/1 flags that never overlap. 26,041 rows have none set, the same count as the missing `raceeth` rows. Use `raceeth` instead. | Clear |
| `marst3`, `marst3_label` | 1 = Married, 2 = Previously married, 3 = Never married. No missing values. | Clear |
| `nchild_cat`, `nchild_cat_label` | 0, 1, 2, or 3+ children. No missing values. | Clear |
| `child_u5`, `child_u5_label` | 0 = No child under 5, 1 = Child under 5. No missing values. | Clear |
| `occ6`, `occ6_label` | 1 = Managers and professionals, 2 = Technical, sales and admin, 3 = Service, 4 = Farming, forestry and fishing, 5 = Craft and repair, 6 = Operators and laborers. 19,838 rows (0.5%) are missing: NIU, military (905), 999, and anything else. By year it is 0.0% in 1976 to 1983, then 0.1% to 1.0%. | Clear |
| `classwkr3`, `classwkr3_label` | 1 = Private, 2 = Government. The name says 3 groups. The third was self-employed, removed on purpose. No missing values. | Clear |
| `region4`, `region4_label` | Census region, 4 groups. Not in the planned tables, so do not load it. | Clear |
| `metro`, `metro_label` | 1 = In a metro area, 0 = Not in a metro area. 122,797 rows (3.3%) are missing. By year: 0.0% in 1976, 12.4% to 17.1% in 1977 to 1985, 1.6% to 1.9% in 1986 to 1995, and 0.3% to 1.1% in 1996 to 2025. So table 11 is weak for 1977 to 1985. | Check |
| `*_label` columns | Text label for the code in the column with the same stem. | Clear |
| `female_0`, `female_1`, `fulltime_0`, `fulltime_1`, and every other `name_number` column | 0/1 dummy columns, one per code. They repeat what the code column already says. Do not load them. | Clear |

Load only the columns we need (use `usecols`), because the file is 1.6 GB.

### Settled
- Year: `year` is the income year.
- Dollars: all `_real` wage columns are in 2025 dollars. No CPI adjustment needed.
- Hourly wage: already built in the file as yearly wage divided by (weeks worked times hours per week). Use `hrwage_real` as it is.
- Full-time: 35 or more hours a week (IPUMS). There is no full-year rule, so a person who worked 1 week at 40 hours counts as full-time. Keep it this way for now.
- Self-employed: removed on purpose, so class of worker has two groups (Private, Government).
- Trimming: one set of cutoffs per year, from all workers, weighted with `asecwt`. Use it in every table, including the full-time sample (user confirmed).
- Hours: `hrswk` is usual hours worked per week last year.
- Race flags: White and Black mean non-Hispanic.
- Missing group values (`raceeth`, `occ6`, `metro`): keep these rows in tables 1 and 2. Leave a row out only of the table grouped by the column it is missing. Print under each table how many rows were left out (user confirmed). The trim cutoffs still come from all workers.
- Table 11: keep 1977 to 1985, with a note that 12.4% to 17.1% of rows have no metro answer in those years (user decided).

### Open questions
- Top codes: check the 4,398 rows at 99 hours, and the extremes of `hrwage_real`, before any math.

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
