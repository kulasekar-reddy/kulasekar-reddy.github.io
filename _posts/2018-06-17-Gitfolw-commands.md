---
layout: post
title: git-flow
description: "Using gitflow helps developers for keeping track on releases and features"
modified: 2018-06-17
comments: true
share:    true
tags: [git,git flow,SCM]
image:
  background: wood.jpg
---

In this post I am going to show you how to use gitflow.

### Installing git-flow on ubuntu
```unixo
sudo apt-get update
sudo apt-get install git-flow
```

### To initialize git-flow on a repositry
```unix
git flow init
```
Select all default values, Because those default values are better for maintaining the git flow.

### Branches

After ```git flow init``` you will be moved to develop branch. 

**master:** This branch contains production ready code.

**develop:** This is the branch where all developers merging their code.

**feature:** This is the branch where individual developer will be working on new features.

**release:** Release branches are useful in giving releases by providing a tag to it.

**hotfix:** hotfix brach is created based on master brach.

### Starting a feature branch

```git flow feature start newfeature```

### To finish the feature branch

```git flow feature finish newfeature```
After finishing the feature branch it will automatically merged to develop branch
Once Devlopment of any feature is completed we have to release it by providing a tag

### Starting a release branch

```git flow release start 1.0.0```
once you start a release branch,tag 1.0.0 will be created based on develop branch code.

### To finish release branch

Once your release is successfully certified by QA,Then finish release branch
```git flow release finish 1.0.0```
After finishing the release branch it will automatically merged to master branch

### Fixing issues in production

To fix issues in production create a hotfix branch,It will be based on master code ```git flow hotfix start bugfix```
After fixing the issue finish the hotfix branch ```git flow hotfix finish bugfix```

### Push tag to remote repositry

```git push origin <tag>```
