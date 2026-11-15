# Git Workflow and Login Test Case Assignment

Public repository: **https://github.com/thrishaldoma/git-workflow-login-assignment**

## Question 1: Git & GitHub Workflow

The project was created locally in `git-workflow-login-assignment`. `index.html` and `about.html` were added before `git init -b main`; the GitHub repository was created only after the local workflow and submission files were complete.

| Assignment step | Implementation and evidence |
|---|---|
| 1. Local setup | Two HTML files; repository initialized locally with `main`. |
| 2. Initial commit | `3fae70e` — Initial commit: add home and about pages. |
| 2. Second commit | `9b3eef9` — Update home page with workflow overview. |
| 2. Branch work | `git branch feature`, then `git switch feature`; `a690868` adds the feature section to `about.html`. |
| 2. Merge | Switch back to `main`; `16fbd7c` merges `feature` using `--no-ff`, retaining a visible two-parent merge commit. |
| 3. Ignore rules | `4f5dbd2` adds `*.log` and `temp/`. `git check-ignore -v` proves that `debug.log` and `temp/scratch.txt` are ignored. |
| 4. Additional commit | `84b3991` adds a temporary paragraph to `index.html`. |
| 4. Undo | `6a04f78` uses `git revert --no-edit HEAD` to remove that paragraph while preserving history. |
| 5. GitHub | The public repository contains both `main` and `feature`; the repository URL appears above. |
| 6. Commands | [git_commands.txt](git_commands.txt) contains the commands and short explanations. |

See [workflow_evidence.txt](workflow_evidence.txt) for captured local command output, the branch graph, ignore checks, and before/after patches for the revert. Immediately after the revert, `git diff --exit-code HEAD~2 HEAD -- index.html` returned exit code 0 with no output, proving the file matched its state before the temporary commit. `git status --short` also returned no output, showing a clean working tree.

The evidence snapshot ends after the undo demonstration. The subsequent documentation commit adds this README, the command documentation, the evidence snapshot, and the test case table. The current full history is available on GitHub or with:

```sh
git log --oneline --graph --decorate --all
```

The local ignored demonstration files are intentionally absent from GitHub. Open `index.html` in a browser to view the small HTML project.

## Question 2: Test Case Design

[login_test_cases.md](login_test_cases.md) contains **14 test cases** with the required four columns: Test Case ID, Test Scenario, Input Data, and Expected Output. It covers all five functional requirements, empty fields, password lengths of 5, 6, and 7, invalid credentials, the lock threshold, and consecutive-failure reset. Test data and assumptions are stated explicitly. These are designed cases; no login-system execution results are claimed.
