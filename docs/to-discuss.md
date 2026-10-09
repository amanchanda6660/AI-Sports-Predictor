# To discuss

Decisions we make as a group. **Vote in the "My votes" section of your own `team/yourname.md` file.** Don't edit this file yourself, since that would cause merge conflicts. Deandre and Adyant count the votes and fill in each **Decision** line after the meeting.

Technical questions (Python version, test tools, and so on) are in [scope-and-definition-of-done.md](scope-and-definition-of-done.md#open-questions-for-the-next-meeting).

## The project in one paragraph

We build a model that predicts who wins a game and gives a probability. We care about whether the probabilities are honest (calibration), not how many games we pick right. The main trap is **data leakage**: accidentally using information from after a game to predict that game, which makes results look great until real games start. The demo is live predictions for this week's games, plus a chart that updates as results come in.

## 1. League

We pick one sport. Each sport's data is very different.

| Option | Good | Not so good |
|---|---|---|
| **A. NFL** (draft plan's pick) | Free data from nflverse, betting odds included, season runs during the semester | Only ~270 games a season, so live results by December will be thin (~8 weeks) |
| **B. NBA** | ~1,230 games a season, so lots of live data; season starts late October | Free betting-odds data is harder to find |
| **C. MLB** | ~2,430 games a season, the most data | Season ends in October, so no live demo until spring |
| **D. European soccer** | football-data.co.uk includes historical betting odds; season runs Aug to May | Draws mean three outcomes instead of two, which is harder |

**Decision:** _open_

## 2. How far up the model ladder

We start simple and add models one at a time, recording how each does. If a simple model beats a fancy one, that's a real finding.

- **A.** Baseline (home team wins X% of the time) + betting market + Elo ratings. Spend the extra time on the app.
- **B.** A, plus logistic regression.
- **C.** B, plus gradient boosting (XGBoost or LightGBM).

**Decision:** _open_

## 3. What the demo shows

The base is a web app showing this week's predictions and a calibration chart that updates as games finish. Pick extras:

- **A.** Base only, done well
- **B.** "Us vs. Vegas": our probabilities next to the betting market's
- **C.** "Why this pick": which inputs pushed each prediction up or down
- **D.** B and C

**Decision:** _open_

## 4. Write-up

- **A.** No write-up. The demo and slides are enough.
- **B.** A short blog post or club write-up of what we found
- **C.** A comparison study of all our models, aiming to publish it

**Decision:** _open_

## 5. Meetings

- **A.** Weekly in person
- **B.** Weekly video call, plus updates in the group chat
- **C.** Group chat only, with a call when something's blocked

**Decision:** _open_

## Who does what

Everyone owns an area and works in its folder. Pick 1 or 2 in your intro. No experience needed for anything marked ⭐.

| Area | What you'd do | Folder |
|---|---|---|
| Data and features | Load game data and build model inputs without leakage | `data/`, `features/` |
| Models | Build Elo, then other models from the ladder | `models/` |
| Evaluation | Score every model the same way (Brier score, log loss, calibration plot) | `eval/` |
| App | Build the web app for the demo | `app/` |
| Market analysis ⭐ | Compare our probabilities with the betting market's | `analysis/` |
| Live tracking ⭐ | Each week, check that predictions were saved before kickoff, and flag any misses | `predictions/` |
| Presentation and write-up ⭐ | Slides, demo script, write-up | `docs/` |

Data, features, models and evaluation are led by Deandre and Adyant. You can still pick them, and you'll pair with one of us.
