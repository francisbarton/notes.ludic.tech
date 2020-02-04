---
title: Exploring git rebase
description: 
date: 2020-01-03
tags:
  - notes
  - #twirl
layout: layouts/post.njk
---

## 

(https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
(https://matthew-brett.github.io/pydagogue/gh_delete_master.html)
(https://blog.scottlowe.org/2015/01/27/using-fork-branch-git-workflow/)

(https://gist.github.com/Chaser324/ce0505fbed06b947d962) :

> ## Keeping Your Fork Up to Date

> While this isn't an absolutely necessary step, if you plan on doing anything more than just a tiny quick fix, you'll want to make sure you keep your fork up to date by tracking the original "upstream" repo that you forked. To do this, you'll need to add a remote:

```
> # Add 'upstream' repo to list of remotes
> git remote add upstream https://github.com/UPSTREAM-USER/ORIGINAL-PROJECT.git
> # Verify the new remote named 'upstream'
git remote -v
```

> Whenever you want to update your fork with the latest upstream changes, you'll need to first fetch the upstream repo's branches and latest commits to bring them into your repository:

```
> # Fetch from upstream remote
> git fetch upstream

> # View all branches, including those from upstream
> git branch -va
```

> Now, checkout your own master branch and merge the upstream repo's master branch:

```
> # Checkout your master branch and merge upstream
> git checkout master
> git merge upstream/master
```

> If there are no unique commits on the local master branch, git will simply perform a fast-forward. However, if you have been making changes on master (in the vast majority of cases you probably shouldn't be - see the next section, you may have to deal with conflicts. When doing so, be careful to respect the changes made upstream.

> Now, your local master branch is up-to-date with everything modified upstream.



[my stackoverflow question about this](https://stackoverflow.com/questions/59592328/what-is-the-recommended-way-of-getting-the-latest-updates-from-upstream-eleventy)