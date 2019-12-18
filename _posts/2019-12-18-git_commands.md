---
title: 'Git Commands'
date: 2019-12-18
permalink: /posts/2019/12/18/git_commands/
tags:
  - git
---

A git repository is a collection of commits. Commits are organized hierarchically (commits have generally one parent, the initial commit has 0 parent, merge commits have generally two parents). A commit has also an author, a date, a committer, a message and the id of a tree.

A tree stores a group of files together (similar to unix directories). This manages filenames, permissions, nested trees and files (named blob). For example:

```bash
[jfbocque@tarkin gitproj] git cat-file -p master^{tree}
040000 tree 2ba9c1eff96a2d03efae6df94d0ec9b709f50d97    ESCAPE_Supervision_Delivery
120000 blob fba579f4290a866397916d1aa58ab03285152c91    Oasis_Infrastructure_Delivery
100644 blob dbaa5006a803246f30bd3fc7d23ad5ba498a93c1    build.xml
040000 tree 29a422c19251aeaeb907175e9b3219a9bed6c616    build
040000 tree e685d91cd7a5d3ffc1a41520391fb3a7f9f862f3    products
040000 tree 44fe786417ed7eb8c2d374f324d285329a94a402    tools
```

objects stored in the repository are either commits, trees or blobs. They are identified by the hash of their content. For these three kinds of objects (commits, trees, blobs), the repository is an immutable database (data are added, but never modified). A result of this immutability is that there is no need to make snapshots or baselines, we just need to write down the hash of a commit or to tag it with a human friendly name. A branch is a pointer to a commit that is updated when a new child commit is added.