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
Not checked yet. I could not read the file header from Drive (the file is too big for the Drive connector). To fill this in, run `print(list(data.columns))` in Colab and paste the output. Then list each column here with what it means. Ask the user about any column that is unclear. Do not guess names or meanings.

| Column | What it means |
|---|---|
| (to fill in) | |

### Open questions
- How is "full-time" defined? Check whether the file already has a full-time flag, and what hours and weeks it uses.
- Is the wage already in 2025 dollars, or do we need to adjust it with a CPI column or table?
- How is the hourly wage built (yearly wage income divided by weeks times usual hours)? Check which variables the file has for it.
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
