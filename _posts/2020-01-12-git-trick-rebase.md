---
type: single
title: "GIT Command - Overwrite Trick"
permalink: /git-overwrite-trick/
excerpt: "A short post to all those who have difficulty with repository hell. This Git command parts the sea and brings peace back to your life"
header:
  image: "/assets/images/git-banner.png"
  teaser: "/assets/images/git-banner.png"
author_profile: false
comments: false
draft: false
---

A short post to all those who have difficulty with repository hell. This Git command parts the sea, brings peace and makes the local repository die for your sins.

Especially in IntelliJ i was faced with this issue numerous times, not being able to switch branches without going through some kind of satanic IntelliJ ritual of saving the repo changes to a shelf.

The solution is here:

**Force overwrite of local files from remote repository:**

```
$ git fetch --all
$ git reset --hard origin/master
```

**To download changes from some other branch**

Use the following command:

```
$ git reset --hard origin/<insert_other_branch>
```

> _`Git fetch` downloads latest updates from remote, but doesn't merge or rebase in local files._

Thank the lord! The repo is resurrected back to its stable form!

### Want to know more, check out the [Git cheat sheet here!](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)
