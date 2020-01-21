---
type: single
title: "GIT Command - Overwrite Trick"
permalink: /typescript-summary/
excerpt: "A summery of my favorite keyboard shortcuts and tricks to boost productivity in the intelliJ IDE"
header:
  image: "/assets/images/git-banner.png"
  teaser: "/assets/images/git-banner.png"
author_profile: false
comments: true
draft: false
---

# Git tricks

Force overwrite of local files from remote repository.

```
$ git fetch --all
$ git reset --hard origin/master
```

To download changes from some other branch, use the following command:

```
$ git reset --hard origin/other_branch
```

_Git fetch downloads latest updates from remote, but doesn't merge or rebase in local files._
