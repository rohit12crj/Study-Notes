✅ https://www.youtube.com/playlist?list=PLdpzxOOAlwvIc1TjTwopNSjRJkzES2ZXk --> Study Only Day 9 to 11 --> Abhishek

✅ https://www.youtube.com/watch?v=VmJpdIOiIaU  -->  Git Interview Questions --> Abhishek

---

✅ git vs github --> Git is Distributed Version Control System & GIthub is Cloud Hosting Platform for Git repo

---

✅ Distributed Version Control System ( DVCS ) vs Centralised Version Control System ( CVCS )

| Feature              | **DVCS**                                              | **CVCS**                                         |
| -------------------- | ----------------------------------------------------- | ------------------------------------------------ |
| Repository           | Every developer has a **full copy of the repository** | Only **one central repository**                  |
| Internet Requirement | Can work **offline**                                  | Mostly requires **connection to central server** |
| Speed                | Faster (local operations)                             | Slower (needs server communication)              |
| Backup               | Multiple backups (each developer copy)                | Single point of failure                          |
| Collaboration        | Developers push/pull changes                          | Developers commit directly to central repo       |
| Example Tools        | Git, Mercurial                                        | Apache Subversion, CVS                           |

---

✅ what is git lfs ?
- Git LFS (Large File Storage) is an extension of Git that allows you to store large files (videos, binaries, datasets, models, etc.) efficiently.
- Instead of storing large files directly in the Git repository, Git LFS stores them in a separate storage and keeps only a pointer file in the Git repo.
- Git LFS stores large files in a separate LFS storage server managed by the Git hosting platform. Platforms like GitHub or GitLab usually store the actual objects in backend object storage such as Amazon S3.
- If someone clones a repo without Git LFS installed they will only download the pointer files instead of the actual large files, and Git will show an error suggesting installing Git LFS.

---

✅ How will u implement Git Lfs?
- Install Git LFS --> git lfs install
- Track Large File Types --> git lfs track "*.zip" --> This creates a .gitattributes file.
- Add and Commit Files 
<img width="488" height="106" alt="image" src="https://github.com/user-attachments/assets/2312f4b4-ad30-4acd-b25c-45b1a511d14b" />

- Push to Remote Repository --> git push origin main

---

✅ What problem happens if Git LFS is not used for large files?
- Repository becomes extremely large
- Slow cloning and pulling
- Poor Git performance

---

✅ Explain Git workflow ? --> Working Directory → Staging Area → Local Repository → Remote Repository

- Working Directory → Where you modify files --> Working directory contains below types of files

| File State | Meaning                         |
| ---------- | ------------------------------- |
| Untracked  | Git doesn't know about the file |
| Modified   | File changed but not staged     |
| Deleted    | File removed                    |

- Staging Area → Where you prepare files for commit --> The Staging Area is an intermediate area where you select which changes will go into the next commit --> git add app.py
- Local Repository -→ Where commits are stored locally --> git commit -m "files added"
- Remote Repository -→ Shared repo on platforms like GitHub --> git push

---

✅ if 2 developers are working on 2 different files there won't be any git conflict . if dev 1 pushes code , & after that dev 2 pushes code , dev 2 would need to do git pull to incorporate changes of dev 1 . Git conflicts occur only when two developers modify the same lines of a file. If they modify different sections of the file, Git can automatically merge the changes without conflict.

---

✅ git merging techniques & when to use which ?

- Fast-Forward Merge

<img width="513" height="414" alt="image" src="https://github.com/user-attachments/assets/2068c668-2636-4de3-9538-e2ab58c537e7" />


<img width="346" height="237" alt="image" src="https://github.com/user-attachments/assets/54e97a68-3e08-44b4-8e64-7afa05065788" />


<img width="506" height="464" alt="image" src="https://github.com/user-attachments/assets/2c57185c-c557-4135-9c23-6998404f1f95" />

<img width="493" height="57" alt="image" src="https://github.com/user-attachments/assets/d60c1ac1-3988-4091-8abf-6199daf3733a" />

---

✅ What is HEAD in Git? explain detached HEAD ? When Detached HEAD is Useful ?

- HEAD is a pointer that refers to the current commit or the latest commit on the current branch.
- Detached HEAD occurs when HEAD points directly to a commit instead of a branch.
- Detached HEAD useful --> Inspect old commits --> Test previous versions

---

✅ What files exist inside .git & .gitignore directory ?
- The .git directory is the internal database of Git.It stores all version history, commits, branches, configuration, and metadata of your repository.
<img width="287" height="182" alt="image" src="https://github.com/user-attachments/assets/2fa15b1d-cf7b-4e2c-a1c2-a178b3908021" />
- .gitignore is a file used to specify which files or directories Git should ignore and not track in the repository.

---

explain ORIG_HEAD, FETCH_HEAD and MERGE_HEAD ?

---

✅ Explain Git internal architecture ?

Git internally stores data as objects identified by SHA hashes. All objects are stored in the .git/objects database. The main object types are:
- Blob → stores file content
- Tree → represents directory structure
- Commit → snapshot of repository referencing tree objects
- Tag → named reference pointing to a specific commit

In Git, all repository data is stored in the .git/objects directory. Git stores this data in two formats:
- Loose objects are individual Git objects stored separately in the .git/objects directory when they are first created.
- Packfiles are compressed collections of many Git objects stored together to optimize storage and improve performance.

These are two ways Git manages storage efficiency and performance. Git periodically converts loose objects into packfiles using garbage collection (git gc).

---

✅ git garbage collection ?
- Git Garbage Collection (git gc) is a process that cleans unnecessary objects in the repository, compresses loose objects into pack files, removes unreachable commits, and optimizes repository performance.

---

✅ Why doesn’t Git delete unreachable commits immediately?

Because Git keeps them temporarily to allow recovery using git reflog, preventing accidental data loss.

---

✅ what are rebase leftovers ?
- Rebase leftovers are temporary files or directories that remain when a Git rebase operation is interrupted or not completed.
- Rebase leftovers often cause CI/CD failures when pipelines run git pull --rebase and the previous rebase was not completed properly.

---

✅ Difference between git pull and git fetch?
- git fetch downloads the latest changes from the remote repository but does not merge them into the local branch.
- git pull downloads the changes and automatically merges them into the current branch.

---

✅ suppose your git is 5 gb ? i want to only clone master branch for quick cloning ? how can i do it ? 
- This performs a shallow clone, downloading only the latest commit of the master branch, which significantly reduces clone time and repository size.
- 
<img width="501" height="188" alt="image" src="https://github.com/user-attachments/assets/2043b401-f548-4d01-b455-dab42b876c3d" />

---

✅ Repo size is 5GB. Clone takes 15 minutes. Fix?

<img width="509" height="95" alt="image" src="https://github.com/user-attachments/assets/6e1a3089-2923-4b4b-bac6-eb40983e0e30" />

---

✅ What is the difference between shallow clone (--depth) and partial clone (--filter=blob:none) ?

<img width="590" height="385" alt="image" src="https://github.com/user-attachments/assets/b8cca9f9-aa07-4487-9199-8fd06de3b118" />

---

✅ What is the difference between pull_request and pull_request_target events?
- pull_request runs workflows using PR branch code with restricted permissions, mainly used for testing and validation.
- pull_request_target runs workflows using target branch code with full permissions, mainly used for PR management tasks like labeling or commenting.

---

✅ what is cherry picking ?
- Cherry-picking is a Git operation that allows you to apply a specific commit from one branch to another branch without merging the entire branch.
- It is commonly used for hotfixes, backporting changes, or selectively applying commits.
- Cherry-picking is useful when you want to apply a specific commit like a bug fix from one branch to another without merging the entire branch history.
  
---

✅ What is git stash?
- Git stash stores changes in a temporary stack and allows developers to clean their working directory without committing unfinished work.
- git stash apply restores the stashed changes without removing them from the stash list.
-  git stash pop restores the changes and removes the stash entry.
-  Many developers prefer git stash apply first because it is safer. If the changes apply successfully, they manually remove the stash.
  
---

✅ Why does Git sometimes say ‘Your local changes would be overwritten by checkout’ and how do you fix it?”
- This error occurs when you try to switch branches while having uncommitted changes that conflict with files in the target branch.
- Git blocks the checkout because switching branches would overwrite your current modifications, causing data loss.
- The issue can be resolved by: committing the changes , stashing the changes or discarding the changes if not needed.

---

✅ How do you undo the last commit? Soft vs Hard reset ?

Undoing the last commit in Git can mean different things depending on what you want to keep:
- keep the changes but remove the commit
- remove both commit and changes
- undo a commit that was already pushed

| Goal                              | Command                   |
| --------------------------------- | ------------------------- |
| Undo commit keep staged changes   | `git reset --soft HEAD~1` |
| Undo commit keep changes unstaged | `git reset HEAD~1`        |
| Undo commit and delete changes    | `git reset --hard HEAD~1` |
| Undo pushed commit safely         | `git revert HEAD`         |
| Edit last commit                  | `git commit --amend`      |

---

✅ How do you revert a merge commit?
- Reverting a merge commit is different from reverting a normal commit because a merge has two parent commits. Git needs to know which parent branch should be considered the mainline.

<img width="520" height="188" alt="image" src="https://github.com/user-attachments/assets/30d947e4-6263-4af5-9927-a68c46bfeb59" />

---

✅ How do you revert a revert commit?

---

✅ What happens if you revert a merge commit and then try to merge the same branch again?

---

✅ Git branching strategy used in companies (GitFlow vs Trunk-Based Development) ?
Companies usually follow two main Git branching strategies depending on team size, release frequency, and CI/CD maturity:
- GitFlow → structured, many long live branches

| Branch            | Purpose                                                      |
| ----------------- | ------------------------------------------------------------ |
| `main` / `master` | production code                                              |
| `develop`         | integration branch                                           |
| `feature/*`       | new features --> Work on feature → merge back to develop.    |
| `release/*`       | prepare release                                              |
| `hotfix/*`        | production fixes --> Fix → merge to both develop & main      |

- Trunk-Based Development (TBD) → minimal branches, continuous integration , Trunk-Based Development uses one main branch (trunk) where developers integrate frequently.Short-lived feature branches may exist but are very short-lived.

| Feature              | GitFlow     | Trunk-Based Development |
| -------------------- | ----------- | ----------------------- |
| Branch complexity    | High        | Low                     |
| Release cycle        | Scheduled   | Continuous              |
| CI/CD compatibility  | Moderate    | Excellent               |
| Merge conflicts      | More likely | Less                    |
| Deployment frequency | Low         | High                    |

---

✅ What is git reflog?
- Git reflog is extremely useful for recovering lost commits after operations like reset, rebase, or branch deletion.

| Feature               | git log | git reflog  |
| --------------------- | ------- | ----------- |
| Shows commit history  | Yes     | Yes         |
| Shows deleted commits | No      | Yes         |
| Shows HEAD movements  | No      | Yes         |
| Used for recovery     | Limited | Very useful |

---

✅ Difference between git reset and git revert?
- git reset moves the branch pointer to a previous commit and rewrites history, so it should only be used for local commits that have not been pushed.
- git revert creates a new commit that reverses a previous commit, preserving history, so it is safe for shared branches and production environments.

---

✅ How do you recover deleted branch?
- If a branch is deleted accidentally, the commits are usually still in Git’s database for some time.
- You can recover the branch using git reflog, which records where HEAD and branch references previously pointed.

---

✅ What is git bisect?
- Git bisect uses a binary search algorithm to efficiently locate the commit that introduced a bug.

---

✅ What are Git Hooks (pre-commit, pre-push) & give example when to use then ?

<img width="563" height="393" alt="image" src="https://github.com/user-attachments/assets/5be03484-9eb5-449c-b759-4e163d59c899" />

---

✅ Someone force-pushed and broke production. What do you do?

---

✅ How does Git ensure data integrity?

---

✅ How does Git handle concurrency?
- Git handles concurrency using optimistic concurrency control, where developers work independently on local copies and conflicts are detected during merges or pushes.
- If multiple developers push changes simultaneously, Git prevents overwriting changes by rejecting non-fast-forward pushes, requiring developers to pull and integrate changes first.

---

✅ Two developers push at the same time — how does Git prevent pipeline duplication and merge conflicts ?

<img width="512" height="268" alt="image" src="https://github.com/user-attachments/assets/1728bb93-5044-431c-844e-2cd1ca1c90d7" />

---

✅ Explain object packing in Git.

---

✅ Difference between index and working tree?

---

✅ How to squash commits?

---

✅ What is sparse checkout?

---

✅ Explain submodules vs subtree.

---

✅ What is orphan branch & orphan commits ?

---

✅ How to sign commits?

---

✅ What is rebase interactive?

---

✅ How do you prevent secret commits? --> use checkov in pipeline

---

✅ What is git blame?

---

✅ How to enforce branch protection?  -> use codeowners file & take manual approvals

---

✅ Why does Git sometimes show ‘detached HEAD’ during git bisect?

---

