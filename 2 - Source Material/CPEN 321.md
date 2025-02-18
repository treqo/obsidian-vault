**Course:** [[]]
**Subject**: #
**Date:** 2025-02-12  
**Instructor:** {{Instructor}}

---
# Version Control Systems

```ad-note
title: Types of VCS
- Git: A distributed VCS
- Apache
```

```ad-summary
title: Distributed Version Control Systems
(e.g. git, mercurial)
**Pros**:
- You do local commits. The full history is always available
- 
**Cons**:
```

````ad-summary
title: Merging
**Two-way merging**
**Three-way Merging**: (git uses 3 way merging)
```sh
git checkout <branch_name>
git merge <branch_to_merge>
```
````

```ad-summary
title: Rebasing
Reapply commits on top of another base tip.
```

````ad-summary
title: Squashing
meld a series of commits down into a single commit.
```sh
git checkout <branch_name>
git commit --squash <branch_to_squash> // way 1
git rebase <branch_to_squash> -i // way 2
```
````

````ad-abstract
title: Cherry-pick
Choose a comit from one branch and apply it to another by creating a new commit
```sh
git checkout master
git cherry-pick <x-SHA> //apply commit x to master branch
```
````



---
## Questions
- 
## Practice Problems 
1. Problem 1
	- Solution
## Additional Resources
- 
