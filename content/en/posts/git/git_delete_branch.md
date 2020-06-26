---
title: "How to Delete a Git Branch Both Locally and Remotely"
date: 2020-06-26T11:30:40+08:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Git
- Tips
series:
- Git
categories:
- 2020-06
image:
---

## TL;DR version
```bash
// delete branch locally
git branch -d localBranchName

// delete branch remotely
git push origin --delete remoteBranchName
```

## Details version
### Delete a LOCAL branch
You can not delete the branch you are currently on so you must make sure to *checkout* a branch that you are NOT deleting. For example:  `git checkout master`.

Deleting a branch with this command `git branch -d branchName`.

>*Note:* The `-d`(Shortcut for `--delete`)option will delete the branch only if it has already been pushed and merged with the remote branch.Use `-D`(Shortcut for `--delete --force`.) instead if you want to force a branch to be deleted regardless of its merge status.

![how to delete a git branch](/img/git_branch_delete.png "How to Delete a Git Branch Both Locally and Remotely")

### Delete a REMOTE branch
If you want to delete a remote branch, use this command `git push <remote> --delete branchName`, you can also use this shorter command to delete a branch remotely: `git push <remote>: branchName`. For example:
```bash
git push origin --delete test
git push origin :test
```

## Reference
https://git-scm.com/docs/git-branch
https://www.educative.io/edpresso/how-to-delete-remote-branches-in-git