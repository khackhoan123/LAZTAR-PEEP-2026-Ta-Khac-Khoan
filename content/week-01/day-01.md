+++
title = "Day 01 - 15/09/2026"
weight = 1
+++

## Topics Learned

### Git

#### Common Commands

| Command            | Meaning                                        |
| ------------------ | ---------------------------------------------- |
| git init           | Create a new Git repository                    |
| git remote         | Manage connections to remote repositories      |
| git clone          | Copy a remote repository to the local machine  |
| git fetch          | Download remote changes without merging them   |
| git pull           | Download and merge remote changes              |
| git status         | Show the current repository state              |
| git branch         | List, create, or delete branches               |
| git switch         | Move to another branch                         |
| git checkout       | Switch branches or restore files               |
| git add            | Stage changes for the next commit              |
| git commit         | Save staged changes to the repository history  |
| git commit --amend | Update the latest commit                       |
| git push           | Upload local commits to a remote repository    |
| git reset          | Unstage changes or move commit history         |
| git rebase         | Reapply commits on top of another branch       |
| git rebase -i      | Edit, squash, or reorder commits interactively |
| git stash          | Temporarily save uncommitted work              |
| git stash pop      | Restore the latest stashed work                |
| git merge          | Combine changes from another branch            |
| git cherry-pick    | Apply a specific commit to the current branch  |

#### Merge Conflict Handling

| Situation                        | Solution in Code Source Control                                     |
| -------------------------------- | ------------------------------------------------------------------- |
| Keep changes from both branches  | Open the file, edit the conflict manually, then mark it as resolved |
| Keep the current branch version  | Use `Accept Current Change` in the conflict editor                  |
| Keep the incoming branch version | Use `Accept Incoming Change` in the conflict editor                 |
| Cancel the merge                 | Open Source Control, use the `...` menu, then choose `Abort Merge`  |
| Resolve conflicts manually       | Review the marked conflict blocks and keep the correct final code   |

---

## Tasks Completed
- Set up communication and project management tools: Slack, Taiga.
- Cloned the trainee template repository, set up Hugo Extended (`v0.166.0`), and initialized the personal notes site.
- Configured repository settings in `config.toml` (`baseURL` and `author`) matching personal GitHub repository `khackhoan123/LAZTAR-PEEP-2026-Ta-Khac-Khoan`.
- Deployed the static notes site to GitHub Pages using automated GitHub Actions workflow.
- Reviewed core Git commands, conflict resolution workflow, and TypeScript/ESLint principles.

## Difficulties & Solutions
- **Issue:** Failed to install Hugo using Windows Package Manager (`winget install Hugo.Hugo.Extended`) due to missing `winget` environment on the machine (`'winget' is not recognized`).
- **Solution:** Switched to a manual PowerShell automation approach: downloaded the official binary package (`hugo_extended_0.166.0_windows-amd64.zip`) from GitHub Releases, extracted it to `C:\Users\takha\bin`, and appended the directory to the Windows User `PATH` environment variable. Verified successfully with `hugo version`.

## Next Plan
- Practice complete Git Flow lifecycle on a sandbox repo (branch creation, PR creation, code review, merge, and simulated conflict resolution).
- Prepare workspace and continue onboarding tasks for Day 02.

## Practice Screenshots

![Git Basic Operations](/images/day01/git-basic.png)

![Git Stash and Merge Conflict Resolution](/images/day01/git-conflict.png)