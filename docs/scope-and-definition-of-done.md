# NFL Predictor: Scope and Definition of Done

**Status:** Draft, Oct 2, 2026. Read it, mark anything you disagree with, and bring it to the next meeting. Once we agree on it, this file is how we decide whether something is done. Changes after that go through a pull request.

## Goal

By mid-December 2026, we demo a Streamlit app. It shows calibrated home-win probabilities for this week's NFL games and a live record of how our past predictions did.

"Calibrated" means that when the model says 70%, the home team wins about 70% of the time. That matters more to us than how many games we pick right. The biggest risk is data leakage, where post-game information slips into pre-game inputs and makes our backtests look better than they really are.

## Scope

### In scope

| Area | What we're doing |
|---|---|
| League and data | NFL from the nflverse data. Seasons 2010–2025 for backtesting; the 2026 season for live predictions. |
| Prediction | One row per game. The output is the probability that the home team wins (`home_win` = 1 or 0). |
| Data splits | Train on 2010–2021, validate on 2022–2023, test on 2024–2025. We split by season and never shuffle across time. |
| Models | (1) Home win rate baseline. (2) Market benchmark from closing moneylines with the vig removed. (3) Elo, using the FiveThirtyEight method. Optional later: logistic regression, XGBoost/LightGBM. |
| Evaluation | Brier score, log loss, and a calibration plot. Every model goes through the same evaluation function. |
| Live tracking | Starting once Elo works, we log predictions with a timestamp before kickoff every week. |
| Automation | A weekly GitHub Actions job reruns the predictions and commits the predictions file. |
| App | A Streamlit app showing this week's predictions and a calibration chart that updates as results come in. |
| Market analysis | Notebooks comparing our probabilities to the betting market. |
| Presentation | Slides and a live demo for the club. |

### Out of scope

- Other leagues and college football
- Predicting point spreads, totals, final scores, or player stats
- Win probability during a game
- A mobile app, a database, or user accounts
- Betting advice or real-money betting. The market analysis compares probabilities and doesn't recommend bets.
- Paid data sources and scraping injury news
- Win rate (the share of games picked right) as our headline number

If something isn't on either list, the team agrees on it before anyone starts work.

## Ground rules

These apply to every piece of work.

1. Every input for a game uses only games played before that game's kickoff.
2. For rolling stats, always `shift(1)` before `rolling()`.
3. Week 1 inputs come only from earlier seasons.
4. We use the test seasons (2024–2025) once, at the end, after the models are frozen.
5. If any model clearly beats the market benchmark, we treat it as a bug and investigate before anyone reports the number.
6. We never backfill live predictions. A missed week stays missed, and we note it in the log.

## Definition of Done

There are three checklists: one for any pull request, one for a model, and one for the whole project.

### A pull request is done when

- [ ] It's a pull request, not a direct push to `main`
- [ ] One of the two coders has approved it. CODEOWNERS requires this for `data/`, `features/`, `models/`, and `eval/`.
- [ ] The tests pass
- [ ] The comments are clear enough that a non-CS teammate can follow what the code does and why
- [ ] It adds no new dependency unless a coder signed off. If one was added, `requirements.txt` is updated.
- [ ] The README or `docs/` is updated if the way to run something changed
- [ ] If it touches features, a leakage test covers the change

### A model is done when

- [ ] It outputs a home win probability for every game, strictly between 0 and 1
- [ ] Every input follows the ground rules above
- [ ] A leakage test recomputes one game's inputs from data cut off at that game's kickoff and gets the same values
- [ ] Its settings were tuned on the train and validation seasons only
- [ ] It has been run through the shared evaluation harness on the validation seasons (2022–2023)
- [ ] Its numbers sit next to the baseline's and the market's in one table
- [ ] Its results land where we expect: better than the home-rate baseline and no better than the market. If they don't, someone investigates before the result is reported.
- [ ] A short note in `docs/` says what the model does, what settings it uses, and its validation numbers
- [ ] The other coder has reviewed it

### The project is done when

- [ ] One command rebuilds the game table from scratch
- [ ] The baseline, market and Elo models have been evaluated through the same harness on validation, then once on test
- [ ] The test results are recorded once, after the models are frozen, and never changed afterward
- [ ] All leakage tests pass
- [ ] The live log has a timestamped, pre-kickoff prediction for every week since Elo went live, and any missed weeks are noted
- [ ] The GitHub Actions job has run on schedule every week since it was set up
- [ ] The app is deployed at a link anyone in the club can open
- [ ] Someone who didn't build the app can run the demo
- [ ] The market analysis compares Elo with the market on the test seasons and on the live 2026 games
- [ ] The README explains how to run everything
- [ ] The presentation covers what we built, the results table, the calibration plots, and the project's limits

**One limit to plan for:** by mid-December we'll have about 8 weeks of live predictions, or roughly 100–130 games. That's enough to show the live chart working, but too few games to judge calibration with confidence. The 2024–2025 test seasons (about 570 games) are our main evidence.

## Milestones (proposed)

| Target date | Milestone | Owner |
|---|---|---|
| Fri Oct 9 | Repo set up; data loader, evaluation harness, and baseline and market results reviewed | Coders |
| Wed Oct 14 | Elo working and reviewed on the validation seasons | Coders |
| Thu Oct 15 | First live predictions logged before that week's first kickoff | Coders and tracker |
| Fri Oct 23 | GitHub Actions job committing predictions weekly | Coders |
| Fri Oct 30 | App v1 deployed with this week's predictions and the calibration chart | App owner |
| Fri Nov 13 | Market analysis notebook v1 | Analysis owner |
| Fri Nov 20 | Optional model rung, only if everything above is done | Coders |
| Fri Dec 4 | Models frozen; test set run once; results recorded | Coders |
| Wed Dec 9 | Presentation draft and demo rehearsal | Everyone |
| Mid-Dec (TBD) | Demo | Everyone |

Each week without live Elo predictions costs us about 14–16 games of live data, so Oct 14 is the date that matters most.

## Roles

All changes go through a pull request approved by one of the two coders.

| Area | Folder | Who |
|---|---|---|
| Data, features, models, evaluation | `data/`, `features/`, `models/`, `eval/` | [Coder 1], [Coder 2] |
| Streamlit app | `app/` | [name] |
| Market and betting analysis | `analysis/` | [name] |
| Live prediction tracking: check each week that predictions were logged before kickoff, and flag any misses | `predictions/` | [name] |
| Notes and presentation | `docs/` | Everyone |
| New member | TBD | [name] |

## Open questions for the next meeting

1. **Data source.** Should we read the nflverse schedule CSV directly with pandas (no new packages), or use `nflreadpy`? `nflreadpy` needs Python 3.10+ and adds polars, pydantic and other packages. *Recommend: the CSV.*
2. **Python version.** We should all use the same version so our setups match. *Recommend: 3.11.*
3. **Ties.** Should we drop them or score them as 0.5? They're rare, about one a season. *Recommend: drop them and record how many we dropped.*
4. **Playoffs and neutral-site games** (international games, the Super Bowl). Should we include them? *Recommend: include them, with no home-field bonus at neutral sites.*
5. **pytest.** Can we add it for the leakage tests? *Recommend: yes.*
6. **Branch protection.** Required reviews on a private repo need GitHub Pro or Team (students can get Pro free through GitHub Education). On a free plan, they only work on public repos. *Recommend: make the repo public. The data is public anyway, and the repo becomes a portfolio piece.*
7. **New member.** Why can't we add them as a GitHub collaborator, and which area will they own? Until they have access, they can review this doc and start on the presentation.
8. **Demo date and room.**
9. **App hosting.** *Recommend: Streamlit Community Cloud. It's free and deploys straight from GitHub.*
10. **What counts as "clearly beats the market"?** *Recommend: if a model scores a lower Brier score or log loss than the market on validation, we investigate it, no matter how small the gap.*

## Glossary

- **Brier score:** The average squared gap between the predicted probability and what actually happened (1 or 0). Lower is better. Always guessing 50% scores 0.25.
- **Log loss:** Like the Brier score, but it punishes confident wrong predictions much harder. Lower is better.
- **Calibration plot:** Groups predictions into ranges, such as 60–70%, and shows how often the home team actually won in each range. A well-calibrated model's points sit on the diagonal line.
- **Vig:** The bookmaker's cut. The probabilities implied by raw moneyline odds add up to more than 100%. Removing the vig scales them back down to 100%.
- **Elo:** A rating system in which teams gain points for wins and lose points for losses. The size of each change depends on opponent strength, home field and margin of victory.
- **Data leakage:** When information from after a game, such as its final score, gets into the inputs used to predict that game. It makes backtests look better than reality.
- **Backtest:** Running a model on past seasons as if we were predicting them at the time.
