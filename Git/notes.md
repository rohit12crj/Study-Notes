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

- 3-Way Merge
- Squash Merge
- Rebase

---

What is HEAD in Git? explain detached HEAD ? When Detached HEAD is Useful ?

What files exist inside .git & .gitignore directory ?

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

Difference between git pull and git fetch?

suppose your git is 5 gb ? i want to only clone master branch for quick cloning ? how can i do it ?

cherry picking

What is the difference between merge and rebase?

What is git stash?

How do you undo the last commit?  --> soft & hard reset

Git branching strategy used in companies (GitFlow vs Trunk-Based Development) ?

What is git reflog?

Difference between git reset and git revert?

How do you recover deleted branch?

What is git bisect?

What are Git Hooks (pre-commit, pre-push) & give example when to use then ?

Explain shallow clone.

Someone force-pushed and broke production. What do you do?

Repo size is 5GB. Clone takes 15 minutes. Fix?

How does Git ensure data integrity?

How does Git handle concurrency?

Explain fast-forward merge.

What happens if two developers push same branch simultaneously?

Explain object packing in Git.

Difference between index and working tree?

How to squash commits?

What is sparse checkout?

Explain submodules vs subtree.

What is orphan branch?

How to sign commits?

What is rebase interactive?

How do you prevent secret commits? --> use checkov in pipeline

What is git blame?

How to enforce branch protection?  -> use codeowners file & take manual approvals

