---
layout: post
title: GitExtensions MOD
tags : [GitExtensions, Git]
keywords: [GitExtensions, Combined Diff, cherry-pick, force-with-lease, ]
category : GitExtensions
---

This MOD is an enhanced version of GitExtensions with the most wanted features and bug fixes flavored, such as combined diff (for reviewing merge conflicts), left branches panel, multiple commits cherry-picking, branches/commits comparision, force with lease, showing real line number in the diff and so on.

Please don't submit any issue to the official team as it's a MOD version. You can report issues [Here](https://github.com/EbenZhang/gitextensions/issues/new) if you have any concern. 

If you want to contribute a fix or feature, please make sure your branch is based on the official master branch, so it can be merged to both official and this MOD.

<!--more-->

* auto-gen TOC:
{:toc}


# Get Started

[Download](https://github.com/EbenZhang/gitextensions/releases/download/2.49.99.1/2.49.99.1_MOD.7z) it, unzip it to overwrite your existing official GitExtensions.


[Here](https://github.com/EbenZhang/gitextensions/releases) is the release page contains the latest binaries and source code.


# Features

## Ability to Review Merge Conflicts

To help figure out what changes had been made while solving the merge conflicts, we introduced a new section in the files panel to show the combined diff.

As you can see from the below image, there is a new combined diff section in the left panel.

![image](/images/EnhancedGitExtensions/CombinedDiff.png)

By clicking any file in the list, you can see the combined diff in the right panel. 

FYI: Combined diff means the differences between the current revision and all of its parents, for example there is a space and together with a plus sign ` +`at line 2102, this indicates the line is a new added one in compare to its right parent. The diff indicator will be `++` if it's a new line for both of its parents).

### Omitting the Uninteresting Diff

By default, it may show some unexpected differences that were not caused by conflicts. Those differences are so called uninteresting diff, by contrast, the differences caused by solving conflicts are interesting diff.

Fortunately, there's an advanced option to omit the uninteresting diff.

![image](/images/EnhancedGitExtensions/omituninterestingdiff.png)

### Known Issues

- Line numbers of parents are not supported. As shown above, only one column of line numbers is showing. The line numbers for its two parents are missing.
- Some context menu items are not supported. E.g `open in external diff tool`
- Showing combined diff for octopus merge is not supported.

## Left Panel for Local Branches

![image](/images/EnhancedGitExtensions/leftpanel.png)

### Known Issues

The remote branches, stashes and tags are not showing, but better than nothing, isn't it?

## Ability to Cherry-pick Multiple Commits

In the revision grid, you're now able to select multiple commits and cherry-pick them. Just keep in mind the first commit selected will be cherry-picked first.

In addition, the choice of whether to automatically create a commit and add reference to the original one will be kindly remembered.

## Real Line Number in the Diff

In this MOD, the diff view shows the real line number of the file.

![image](/images/EnhancedGitExtensions/RealLineNum.png)

### Known Issues

- Combined diff for merge commit that has two parents is partially supported as described earlier.
- Showing real line number of octopus merge commit is purely not supported.

## Force-with-lease

`Force-With-Lease` is a new introduced (in git 1.85) successor of `force push` as it will make sure there is no change made by other people on the remote side; thereby protecting you from inadvertently overwriting other people's commits.

![image](/images/EnhancedGitExtensions/forcewithlease.png)

## Comparing to Another Branch

Using this handy feature, you're able to compare a commit (or branch) to another, therefore easily review the changes on your branch.

The feature is accessible from the context menu:

![image](/images/EnhancedGitExtensions/CompareBranches.png)

Then you'll see the differences in a popup dialog like below.

![image](/images/EnhancedGitExtensions/CompareBranchesDetails.png)

As you may already aware of, there is a `Compare right commit to merge base` option. With it ticked, you're able to compare a branch to the most recent common commit between the left and right commits instead of comparing to the left one directly. This is useful if your branch was created based on an old commit of the target branch to where your branch is going to be merged.

In addition, there is a `swap` button which swaps the left and right commit when confusing of which commit is the base/head.

Moreover, you can pick up another branch to compare by clicking the `Another Branch` button.

## Known Issues

Translation hasn't been supported. 

## Remembering the `ignore white space` settings

The toolbar button highlighted below can ignore the white spaces changes in the diff view.

![image](/images/EnhancedGitExtensions/dontshowwhitespacetoolbar.png)

If you want, you can tick an advanced option to remember you choice.

![image](/images/EnhancedGitExtensions/omitwhitespacesettings.png)

## Option to NOT Save Local Changes Preference When Checking Out

The original GitExtensions will remember the dangerous choice (the reset) made on the `Checkout branch` dialog, which might cause people lose all the local changes next time.

With this MOD the choice on the `Checkout branch` dialog will be saved only when the `Set as default` check box is ticked.

![image](/images/EnhancedGitExtensions/LocalChangesDefaultAction.png)

## Auto Stash before Rebase

This feature automatically performs a stash if you have any uncommitted changes before rebasing and then automatically pop the stash afterwards.

To use this feature, you'll need to tick the `AutoStash` in the advanced rebase options.

![image](/images/EnhancedGitExtensions/rebaseOptions.png)

![image](/images/EnhancedGitExtensions/autostach.png)


# Bug Fixes

## Unable to Restore Once Minimised

When having two or more GitExtensions instances running on Win7 or later,  you might not be able to restore them after minimised by Win+D or Win+M.

## Intermittently ObjectDisposedExceptio

When checking if there is a upgrade version of GitExtensions, the program may crash intermittently with an ObjectDisposedException.

## Statistics Plugin Calculates Files Not Under Source Control

Now the statistics plugin only counts the number of lines for files that under source control.
