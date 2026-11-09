# Section 7: Git and GitHub Practice

Student: **Replace with your name**

Research question: What share of registered voters voted across these eight fictional precincts?

This is an ungraded practice project for POLISCI 150A. Every observation is invented for this exercise. 

## What is here?

| File | Purpose |
|---|---|
| `analysis.Rmd` | A short report to edit and knit |
| `data/turnout.csv` | Eight invented precincts |
| `AI_LOG.csv` | A record of the simulated suggestion and your verification |
| `ASSETS.csv` | File origins and verification status |
| `prompts/simulated_suggestion.txt` | A deliberately incorrect suggestion supplied for class |
| `.gitignore` | Excludes local R state, credentials, and temporary files |
| `section7-sandbox.Rproj` | Opens the project in RStudio |

## Start with your own copy

1. On the TA's GitHub repository, click **Use this template → Create a new repository**.
2. Select your own GitHub account as owner. Name the repository `section7-practice-YOURUSERNAME`. Choose **Private**.
3. In your new repository, click **Code → HTTPS** and copy the URL.
4. In RStudio, choose **File → New Project → Version Control → Git**, paste that URL, and create the project.
5. Open Git History. A template copy starts with one initial commit; it does not inherit the TA template's earlier history.
6. Open `analysis.Rmd` and click **Knit** before editing it. The output is HTML, so this exercise does not require a LaTeX installation.

If R says `rmarkdown` is missing, run `install.packages("rmarkdown")` in the **R Console**. RStudio supplies Pandoc in its normal installation.

Use your own repository for edits and pushes. You do not need write access to the TA's template.

## Exercise 1: Edit, inspect, stage, commit, push

1. Replace the name placeholder in this README.
2. Put your name in the `author:` field at the top of `analysis.Rmd`.
3. Knit and save the report.
4. In the Git pane, read the diff. Stage `README.md`, `analysis.Rmd`, and the generated `analysis.html`.
5. Commit: `Fill in README and author`.
6. Push, refresh your GitHub repository, and read the commit's diff there.
7. Show a partner the history. Can they tell what changed and when?

## Exercise 2: A working checkpoint

In the empty **Load and inspect the data** code chunk of `analysis.Rmd`, add:

```r
turnout <- read.csv("data/turnout.csv")
nrow(turnout)
```

Knit. You should see **8**, the number of precincts in the CSV. Compare it with the CSV's eight data rows.

Read the diff, stage the report source and HTML, and commit: `Load data`. This is the working checkpoint you will recover later.

## Exercise 3: Break it, then recover it

This is a simulation using supplied text. Do not contact an AI service for this exercise.

1. Make sure your working changes are committed. `git status` should report a clean working tree.
2. Open `prompts/simulated_suggestion.txt`. It proposes the wrong file path.
3. In `analysis.Rmd`, replace **only** `data/turnout.csv` with `data/voters.csv`.
4. Save, inspect the one-line diff, and commit the Rmd as `Apply suggested fix (unverified)`. For this demonstration we intentionally record the broken version before testing it.
5. Knit. It should fail because `data/voters.csv` does not exist.
6. In History, find the **Load data** commit and copy its hash.
7. In RStudio's **Terminal tab**, first run `git status` and check that you are in this practice project. Then type the following, replacing `GOOD_HASH` with the actual working commit's hash:

```bash
git checkout GOOD_HASH -- analysis.Rmd
```

Do not type the word `GOOD_HASH` literally. This replaces `analysis.Rmd` with its version from that commit and stages the restored file. It leaves later commits in history. It can overwrite current edits in this file, so do it only after committing your intended work, as above.

8. If RStudio asks whether to reload the changed file, accept. Knit again. The row count should be **8**.
9. Read the diff and commit the restored source and refreshed HTML, if changed: `Restore working load chunk`.
10. Push. Look for the three checkpoints: working → broken → restored.

## Exercise 4: Verify and document

1. In the final code chunk, calculate totals and turnout:

```r
total_registered <- sum(turnout$registered_voters)
total_votes <- sum(turnout$votes_cast)
turnout_pct <- 100 * total_votes / total_registered
c(registered = total_registered, votes = total_votes,
  turnout_percent = turnout_pct)
```

2. Verify independently by adding the values in `data/turnout.csv`: **800 registered voters**, **520 votes**, and **65% turnout**. A calculator or hand calculation is enough.
3. Write two or three sentences under **Verification note**: what failed, how you checked the real filename and results, and what you restored.
4. Add one row to `AI_LOG.csv`. Identify this as a **TA-supplied simulation, no live AI**, reference `prompts/simulated_suggestion.txt`, and describe the error and verification.
5. Update the relevant statuses in `ASSETS.csv`. A filename or a successful knit alone does not prove an analysis is correct.
6. Knit, inspect the changes, and commit the report, HTML, and logs: `Verify turnout and document simulated error`. Push.

## The everyday loop

Pull → edit and knit → inspect diff → stage → commit → push.

Saving is not committing. Committing is not pushing. To practice **Pull**, follow your TA's demonstration with a browser edit in your own repository.

## Discuss before leaving

1. Before accepting a large suggested change, why commit a clean, working state?
2. What would someone be unable to learn from a history with just one commit called `final`?
3. Can an analysis be reproducible and still wrong?

## Keep out of Git history

Never commit tokens or passwords. `.gitignore` excludes `.Renviron`, `.Rhistory`, `.RData`, and `.Rproj.user/`. Ignore sensitive files before tracking them; ignoring an already tracked file does not remove it or its past contents from history.

