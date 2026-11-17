# Exercises: Day 3

Collaborative software development: branches, pull requests, code review and CI.

> **Morning exercises (1-4)** are done alone, in **your own fork** from Day 2. Nothing from your Day 2 work is lost: we only add branches.
> **Afternoon exercises (5-8)** are done in your **team repo** in the course GitHub organization.

## 1 💪 Create, switch and merge a branch  (~15')

- Open your Day 2 project in VS Code. Make sure you are on `main` and `git status` shows a clean working tree
- Create a new branch called `practice/<your-github-username>`
- Create a new file `notes.md` with one line of text in it and commit it with the message `Add notes`
- Switch back to `main`. What happened to `notes.md`?
- Switch to your practice branch again. What happened to `notes.md`?
- Switch to `main` and merge your practice branch into it
- Open the `GRAPH` in the Source Control view. Did git create a *merge commit*?
- Delete the practice branch
- Don't push anything

<details>
<summary>🔍 Click here for hints! </summary>

- Branch picker: click the branch name at the bottom left of VS Code → *Create new branch...*
- Terminal equivalent: `git switch -c practice/yourname`
- Switch branches: click the branch name, or `git switch main`
- Merge: *Source Control* view → `...` → *Branch* → *Merge...*, or `git merge practice/yourname` (run it while standing on `main`)
- Delete: `git branch -d practice/yourname`
- Look at the history in the terminal: `git log --oneline --graph --all`
</details>

<details>
<summary>🔍 Why was there no merge commit? </summary>

`main` had not moved since you created your branch, so git could just slide the `main` pointer forward to your commit. This is called a **fast-forward** merge. A merge commit is only needed when both branches have new commits (you will see this in the next exercise).
</details>

#### 🚀 Optional challenge

Repeat the exercise, but this time commit something to `main` *before* merging your practice branch. Compare the graph with the first time.

## 2 💪 Merge conflicts  (~20')

- Start on a clean `main`
- Create two branches from `main`: `conflict-a` and `conflict-b`
- Pick **one line** of `README.md` (for example the title)
- On `conflict-a`, change that line to something, and commit
- Switch to `conflict-b`, change **the same line** to something *different*, and commit
- Switch to `main` and merge `conflict-a`. This should work without problems
- Now merge `conflict-b` into `main`. 💥 This time git will stop and report a conflict
- Look at `README.md` in the editor. Find the lines starting with `<<<<<<<`, `=======` and `>>>>>>>`
- Resolve the conflict using VS Code's merge editor. Keep a version that makes sense, not just the first one you see
- Save the file, run the tests, stage the file and commit to finish the merge
- Look at the graph. Can you see the merge commit now?
- Turn to your neighbour and explain which version you kept, and why
- Delete both branches

<details>
<summary>🔍 Click here for hints! </summary>

- Create a branch **from main**: make sure you are on `main` first, then `git switch -c conflict-a`; go back with `git switch main` before creating `conflict-b`
- Merge: `git merge conflict-a`
- Conflicted files are listed in *Source Control* under **Merge Changes**. Click one, then **Resolve in Merge Editor**
- The buttons *Accept Current*, *Accept Incoming* and *Accept Both* keep your side, the other side, or both. You can also edit the result by hand in the bottom pane
- If you get lost: `git merge --abort` takes you back to before the merge
- Delete branches: `git branch -d conflict-a conflict-b`
</details>

<details>
<summary>🔍 Why can't git decide by itself? </summary>

Git merges changes line by line. When two branches changed *different* lines, it can combine them. When they changed the *same* line differently, only a human knows which version (or combination) is right. A conflict is not an error: it is git asking for your judgement.
</details>

## 3 💪 Choose a branching strategy  (~15')

Work in your new team. For each scenario, agree on a recommendation and be ready to defend it:

| Scenario | Where does work happen? | Who can merge into `main`? | How are versions marked? | Is review required? |
|---|---|---|---|---|
| **A.** A 3-person lab sharing one analysis pipeline | | | | |
| **B.** An open-source package with many outside contributors | | | | |
| **C.** A pipeline that must be frozen and reproducible for each publication | | | | |

Then decide: would you use trunk-based development, GitHub Flow, Git Flow, or a forking workflow for each?

<details>
<summary>🔍 One possible answer </summary>

- **A.** GitHub Flow: short-lived feature branches in the shared repo, a PR for every change, `main` protected. Any member can merge once approved. Tags for important versions.
- **B.** Forking workflow on top of GitHub Flow: outsiders have no write access, so they fork and send PRs. Maintainers review and merge.
- **C.** GitHub Flow plus a **tag or release per publication** (e.g. `paper-2026-v1`). A long-lived release branch is only needed if you must also patch old versions.
- Git Flow (`develop` + release branches) is rarely worth the overhead for research code unless you ship scheduled releases to many users.
</details>

## 4 💪 Your first pull request  (~15')

- In your own fork, create a branch `docs/readme-tweak`
- Change one line of `README.md` (fix a typo, add a sentence...), commit and publish the branch to GitHub
- Open your repository on github.com. Click **Compare & pull request**
- ⚠️ **Before you click anything else**: look at the *base repository* at the top of the page. It probably points to the *original* repository you forked from. Change it to **your own fork**, with base branch `main`
- Write a title and a description: **what** you changed and **why**
- Click on the **Files changed** tab and read your own diff as if somebody else wrote it
- Merge the pull request, then delete the branch on GitHub
- Locally: switch to `main` and pull. Confirm your change is there

<details>
<summary>🔍 Click here for hints! </summary>

- Publish a branch from VS Code: *Source Control* → **Publish Branch**, or `git push -u origin docs/readme-tweak`
- The **base repository** dropdown sits just under the title *Open a pull request*
- Update your local `main`: `git switch main` then `git pull`
</details>

<details>
<summary>🔍 What happens if I forget to change the base repository? </summary>

The pull request is opened against the original repository, and its owners get a notification about a change they didn't ask for. If it happens: close the PR (*Close pull request*) and open a new one with the right base.
</details>

## 5 💪 Round 1: Fork and pull request  (~30')

Roles in your team: **one Maintainer** (the person whose birthday comes next) and **one or two Contributors**. You will swap roles later, so pay attention to what each role can and cannot do.

The team repo has issues labelled `round-1`. Contributors **do not have write access** to the repo, so they must work through a fork.

#### 🧑‍🔧 If you are the Maintainer

- Open your team repo on github.com and read the issues labelled `round-1`
- Make sure **each contributor has one issue**: assign it (if GitHub won't let you assign someone, ask them to comment "I'll take this")
- While the contributors work, prepare for Round 2 (see the box below)
- When a pull request arrives:
  - Read the description and open the **Files changed** tab
  - Leave **at least one comment** on a specific line (a question or a suggestion)
  - Merge the pull request when you are happy with it
  - Check that the linked issue was closed automatically

#### 👩‍💻 If you are a Contributor

- Open the team repo and click **Fork** (create it under your own account)
- Clone **your fork** into a new folder (not inside your Day 2 project)
- Create a branch called `fix-issue-N` (N is your issue number)
- Make the change the issue asks for
- Commit with a message that includes `Fixes #N`
- Push the branch to **your fork**
- On github.com click **Compare & pull request**. Check that the **base repository is the team repo** and the base branch is `main`. Write a clear description
- Wait for the maintainer's comments. If asked for changes, push more commits to the **same branch**: the PR updates by itself
- After your PR is merged, bring the change back into your fork:
  - Add the team repo as a second remote called `upstream`
  - Fetch and merge `upstream/main` into your local `main`
  - Push `main` to your fork

> 📦 **Prepare for Round 2 (everyone, while waiting)**
>
> You will need a working copy of the **team repo itself** (not a fork) where you can run the tests:
>
> ```bash
> cd ..                                                  # leave your current clone
> git clone git@github.com:ORG/TEAM-REPO.git team-repo   # use the URL on the slide
> source python-intermediate-inflammation/my_env/bin/activate   # Windows: .../my_env/Scripts/activate
> cd team-repo
> python -m pip install -r requirements.txt --editable .  # point your environment at this copy
> python -m pytest                                        # should be green ✅
> ```

<details>
<summary>🔍 Click here for hints! </summary>

- Add the second remote: `git remote add upstream git@github.com:ORG/TEAM-REPO.git`. Check it with `git remote -v`
- Bring in the merged changes: `git fetch upstream`, `git switch main`, `git merge upstream/main`, `git push origin main`
- Commit message example: `Add usage section to README. Fixes #1`
- If you picked the wrong base repository in the PR: close it and open a new one
- Fork icon missing? You are probably looking at your own repo or fork already
</details>

## 6 💪 Review a pull request  (~20')

Your team repo has **one open pull request**, opened by the instructor. Imagine a colleague asks you to review it.

**Individually (10')**

- Read the description, then open **Files changed**
- Go through the checklist: **correctness · tests · readability · scope · docs**. Leave style to the linter
- Write down your findings. For each one note *where* it is and whether it is a `nit:`, a `question:` or `blocking:`
- Bonus: check the branch out locally and *run* the new code. Does it behave as the description says?

**As a team (10')**

- Compare findings and agree on the most important ones
- **One** person writes the team's review on GitHub: add inline comments on the relevant lines (try the **Add a suggestion** button for at least one) and submit the review with **Request changes**, with a short summary
- Everyone else reads the review. Would you feel fine receiving it? Is every comment kind and specific?
- **Do not merge** the pull request. Close it when you're done

<details>
<summary>🔍 Click here for hints! </summary>

- Start a review: on a line in *Files changed*, click the blue **+** and choose **Start a review**. Finish with the green **Review changes** button
- The three outcomes of a review: **Comment**, **Approve**, **Request changes**
- Check out the branch locally: `git fetch origin planted-pr` then `git switch planted-pr`
- Use `small-01.csv` and `inflammation-01.csv` from the `data` folder to try the new function
- Good comment: "`nit:` `x` doesn't say what the variable holds. Maybe `inflammation_data`?" Less good: "bad name"
</details>

## 7 💪 Add CI and protect `main`  (~10')

The **Maintainer drives**; teammates watch and review.

#### Maintainer

- Create a branch `add-ci`
- Move the file `docs/ci.yml.example` to `.github/workflows/ci.yml` (create the folders if needed)
- Commit, push and open a pull request into `main`
- Open the **Checks** tab (or the status at the bottom of the PR) and watch the workflow run
- Ask a teammate to **review and approve** it, then merge
- Create a **ruleset** for `main`: *Settings → Rules → Rulesets → New branch ruleset*
  - name: `protect-main`, enforcement: **Active**, target: **Default branch**
  - require a pull request before merging, with **1 approval**
  - require status checks to pass, and add the check named `test`
  - block force pushes
- Verify it works: try to push directly to `main` from your computer. It should be **rejected**

#### Teammates

- Read `ci.yml` in the pull request and answer: *When does it run? What does it do? What happens when a test fails?*
- Review and approve the PR
- After it's merged: `git pull` and find the workflow file in your copy

<details>
<summary>🔍 Click here for hints! </summary>

- `git mv docs/ci.yml.example .github/workflows/ci.yml` (create `.github/workflows` first with `mkdir -p`)
- The status check only appears in the list of required checks **after it has run once**. This is why we merge the CI pull request *before* creating the ruleset
- If your direct push is *not* rejected: check that the ruleset's enforcement is **Active** and the bypass list is empty
</details>

## 8 💪 Round 2: Team sprint  (~35')

You now all have **write access** to the team repo, and `main` is protected. Nobody can push to it directly, so everything goes through a pull request that needs **1 approval** and a **green CI check**.

Each team member takes **one** issue labelled `round-2`: `daily_median`, `daily_std`, `daily_range` or `daily_sum`. The maintainer role now only matters for merging: **whoever's PR is approved merges it**.

#### Author (≈15')

- In your team-repo copy: `git switch main`, then `git pull`
- Create a branch `feature/<function-name>`
- Implement the function in `inflammation/models.py`, in the same style as `daily_mean` (with a docstring!)
- Add a parametrized test in `tests/test_models.py`
- Add one line describing your change under **Unreleased** in `CHANGELOG.md`
- Run `python -m pytest` and `ruff check .`
- Commit, publish the branch (`git push -u origin feature/<function-name>`) and open a pull request. Fill in the PR template and link your issue with `Closes #N`
- Request a review from the teammate **after you** in the team circle (A → B → C → A)

#### Reviewer (≈10')

- Review the PR you were asked to review, using the checklist from Exercise 6
- Check the CI result at the bottom of the PR. Does it agree with your impression?
- **Approve**, or **Request changes** if there is something blocking

#### Merge (≈5')

- Once a PR is approved and green, the author clicks **Merge pull request**
- Merged first? Great. For the others: 💥 **"This branch has conflicts that must be resolved"**
  - Switch to your feature branch and bring `main` in: `git fetch origin`, then `git merge origin/main`
  - Resolve the conflicts in the merge editor (keep **both** functions, **both** tests, and **both** changelog lines)
  - Run the tests again, commit, push. The PR and the checks update by themselves

#### Aim for

- ✅ **Must**: open a PR and review a PR
- 🟡 **Should**: get your PR merged into the protected `main`
- 🔵 **Could**: resolve the conflict in the second and third PR

<details>
<summary>🔍 Click here for hints! </summary>

- Look at `daily_max` for the pattern: `np.max(data, axis=0)`
- Which axis? We want one value *per day* (one value per column)
- Update an existing branch with the newest `main`: `git fetch origin` then `git merge origin/main`
- Ruff can fix formatting for you: `ruff format .`, then check the changes in the Source Control view before you commit
- Can't approve your own PR? That's the rule working as intended. Ask your teammate
- Stuck on a conflict: `git merge --abort` gets you back to before the merge
</details>

## 🏁 After the exercises

- Which role felt most natural? Which one felt strange?
- When would a review have caught a bug that your tests missed? And the other way round?
- What would change about today's workflow if your team had 20 people instead of 3?
