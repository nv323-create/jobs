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
- Data content: CPS ASEC 1976-2025 from IPUMS. Ages 16-64, worked last year, positive wages. Weight: `asecwt`.
- Main sample: full-time workers.
- Hourly wage: in 2025 dollars. Trim the bottom 1% and top 1% within each year, with men and women together (one cutoff per year, not one per sex).
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
Header read in Colab on 2026-11-05. The file has 85 columns. "Clear" means the name is plain. "Check" means I am not sure of the meaning, so do not use it until it is confirmed. The code to label pairs for coded columns are not listed yet. Print them in Colab first.

| Column | What it means | Status |
|---|---|---|
| `year` | Year of the observation. The ASEC asks about income in the year before the survey, so check whether this is the survey year or the income year. | Check |
| `period`, `period_label` | A period group with 6 values. Probably blocks of years. | Check |
| `asecwt` | Person-level survey weight. Use it in every table and chart. | Clear |
| `age` | Age in years. | Clear |
| `agegrp`, `agegrp_label` | Age group, 5 values. Cutoffs not known yet. | Check |
| `female`, `female_label` | Sex. Used for the gap. The label column shows which code is which. | Clear |
| `fulltime`, `fulltime_label` | Full-time flag (1 or 0). The hours and weeks cutoffs are not known yet. | Check |
| `wkswork` | Weeks worked last year, going by the name. | Check |
| `hrswk` | Hours worked per week. Not known if usual or actual. | Check |
| `incwage_real` | Yearly wage and salary income, in real dollars. The base year is not known. | Check |
| `wkearn_real` | Weekly earnings, in real dollars. The base year is not known. | Check |
| `hrwage_real` | Hourly wage, in real dollars. The base year is not known (we need 2025). Not known if it is already trimmed. | Check |
| `educ5`, `educ5_label` | Education, 5 groups. | Clear |
| `raceeth`, `raceeth_label` | Race and ethnicity, 4 groups. Use this one for the race table. | Clear |
| `hispanic`, `white`, `black`, `other_race` | Probably 0/1 flags that sit behind `raceeth`. Not known if they overlap. | Check |
| `marst3`, `marst3_label` | Marital status, 3 groups. | Clear |
| `nchild_cat`, `nchild_cat_label` | Number of children, in groups 0 to 3. Not known if 3 means "3 or more". | Check |
| `child_u5`, `child_u5_label` | Has a child under 5 (1 or 0). | Clear |
| `occ6`, `occ6_label` | Occupation, 6 groups. | Clear |
| `classwkr3`, `classwkr3_label` | Class of worker, 3 groups going by the name. Only `classwkr3_1` and `classwkr3_2` exist as 0/1 columns, so check how many values it has. | Check |
| `region4`, `region4_label` | Census region, 4 groups. Not in the planned tables. | Clear |
| `metro`, `metro_label` | Metro vs non-metro (1 or 0). The label column shows which is which. | Clear |
| `*_label` columns | Text label for the code in the column with the same stem. | Clear |
| `female_0`, `female_1`, `fulltime_0`, `fulltime_1`, and every other `name_number` column | 0/1 dummy columns, one per code. They repeat what the code column already says. Do not load them. | Clear |

Load only the columns we need (use `usecols`), because the file is 1.6 GB.

### Open questions
- Full-time: the file has a `fulltime` flag. Find out what hours and weeks it uses (look at the min of `hrswk` and `wkswork` when `fulltime` is 1).
- 2025 dollars: the file has `incwage_real`, `wkearn_real`, and `hrwage_real`. Find out the base year. If it is not 2025, adjust it ourselves.
- Hourly wage: `hrwage_real` is already built. Find out how (yearly wage divided by weeks times hours?) and check it against the other columns.
- Check for top-coded and missing values on every wage, hours, and weeks variable before any math.

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
