# Onboarding: your first pull request

**Your task:** add a short intro about yourself to the `team/` folder. Every change to this project follows these same steps, so this is your practice run. You can do all of it in your web browser.

## Words you'll see

- **Repo:** this project, stored on GitHub.
- **`main`:** the official version of the project. Nobody edits it directly.
- **Branch:** your own copy of the project, where you can make changes safely.
- **Commit:** a saved snapshot of your changes, with a short message saying what changed.
- **Pull request (PR):** a request to add your branch's changes into `main`. Someone reviews it first.
- **Merge:** what happens when your PR is approved. Your changes become part of `main`.

## Before you start

1. Make a free account at [github.com](https://github.com). Pick a username you'd be fine putting on a resume.
2. Send your username to Deandre or Adyant in the group chat.
3. Accept the invite that GitHub emails you. You can also find it at [github.com/notifications](https://github.com/notifications).

## Step 1: Read two files (5 min)

- [README.md](README.md) for the team rules
- [docs/to-discuss.md](docs/to-discuss.md) for the decisions we're making. You'll vote on them in your intro.

## Step 2: Copy the template

1. On GitHub, open [team/_TEMPLATE.md](team/_TEMPLATE.md).
2. Click the **Copy raw file** button (two overlapping squares, top right of the file).

## Step 3: Create your file

1. Go back to the `team` folder.
2. Click **Add file → Create new file**.
3. Name the file your first name in lowercase, ending in `.md`, for example `maya.md`. If someone already has your first name, add your last initial: `maya-r.md`.
4. Paste the template and fill it in. Replace everything in `<angle brackets>`. Click **Preview** to see how it will look.

## Step 4: Commit and open your PR

1. Click **Commit changes...** (the green button, top right).
2. For the commit message, write `Add <your name> intro`.
3. Choose **Create a new branch for this commit and start a pull request**. Name the branch `yourname/intro`.
4. Click **Propose changes**, then **Create pull request** on the next page.

## Step 5: Get reviewed

- Deandre or Adyant will review your PR. They might ask you to change something. That's normal, and it's part of the practice.
- To make a change, open your PR, go to the **Files changed** tab, click **⋯** on your file, then **Edit file**. Commit to the same branch (the default option). Your PR updates on its own.
- Once it's approved, the reviewer merges it. You're officially part of the project.

---

## Later: doing the same thing from your computer

You'll need this once we start writing code. Install [Git](https://git-scm.com/downloads) and [VS Code](https://code.visualstudio.com/), open a terminal, and run these one at a time:

```bash
git clone https://github.com/amanchanda6660/AI-Sports-Predictor.git
cd AI-Sports-Predictor
git switch -c yourname/practice          # make a new branch
# edit your team file in VS Code and save it
git add team/yourname.md                 # choose what to include
git commit -m "Update Maya intro"        # save a snapshot
git push -u origin yourname/practice     # upload your branch
```

The last command prints a link. Open it and click **Create pull request**.

**Every time you start new work**, get the latest `main` first:

```bash
git switch main
git pull
git switch -c yourname/new-thing
```
