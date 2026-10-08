# CLAUDE.md

## About this project
- This is a course research project.
- The data is CPS ASEC from IPUMS.
- The analysis lives in a Python notebook (.ipynb).
- The notebook runs in Google Colab.

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
- Weight results with the CPS survey weight (ASECWT) when you report totals or averages.
- Check for missing and top-coded values (like 99999999) before any math.
- Don't commit large data files or anything with restricted data terms. IPUMS data can't be redistributed.

## How to explain changes
- Explain every change in plain English.
- Say what you changed, why you changed it, and what it does to the results.
- Show the logic step by step, like you're explaining it to a friend.
- If you're not sure about something, say so. Don't guess.

## Git rules
- Work on the designated feature branch.
- Commit with a clear message that says what changed.
- When the work is done, push the branch and merge it into main.
- Don't open pull requests.
