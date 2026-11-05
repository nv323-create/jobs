# LESSONS

Short notes on past mistakes and preferences. Read these at the start of every session.
Format: what happened, then the rule for next time.

## Corrections and preferences

- I guessed uppercase IPUMS column names (YEAR, ASECWT) and they were wrong. Rule: all variables are lower case. Ask or check before guessing column names.
- You want me to have the latest repo files. Rule: sync to main (git pull origin main) before starting work.
- You set chart rules: print the weighted table first, chart matches it, units on axis labels, y-axis includes 0. Rule: follow the Chart rules in CLAUDE.md.
- You want the unweighted number of people (n) in the group tables. Rule: tables 3 to 10 show n for women and men next to each gap (see Planned tables in CLAUDE.md).
- You want the notebook to look like the course sample notebook (Python Workshop Week 10): plain short cells, a Variable | Meaning table, labels with `.map()` plus a crosstab check, and a **Question.** line in each section. I over-built it first with extra dictionaries and print loops. Rule: copy the sample's style and keep it simple.

## Code errors and causes

- I gave a Colab cell that read a Drive file but did not mount Drive, so it threw FileNotFoundError. Rule: every cell I hand over must run in a fresh Colab session, so include `drive.mount` before any `/content/drive` path.
- My check cell left out `educ5`, `marst3`, `occ6`, and `child_u5`, so their codes never printed. Rule: before I hand over a check cell, list every column the planned tables use and cover all of them.
- An edit script of mine failed (its text did not match), but the next line committed anyway, so a commit went out without that change. Rule: put `&&` right after the edit script (`python3 - <<'EOF' && git add ...`), because `set -e` did not stop it either, and run `git pull` and read `git log` right before I push, because another run can commit in between. It happened twice.
