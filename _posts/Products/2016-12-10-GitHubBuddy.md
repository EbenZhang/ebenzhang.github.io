---
layout: post
title: "GitHubBuddy"
description: "A browser extension to make GitHub better"
category : Products 
tags : [GitHub, Products]
keywords: [DiffTool, Wide GitHub, JIRA, TFS, GitHub]
---

{% capture githubbuddy %}{% include githubbuddy_excerpt.md %}{% endcapture %}
{{ githubbuddy | markdownify }}


<!--more-->

## How to Install and Configure

Currently, it only supports FireFox and Chrome on a Windows system. You can follow the below steps to install it:

### Install

- Install the free [FireFox Extension](https://addons.mozilla.org/en-US/firefox/addon/githubbuddy) or the [Chrome Extension](https://chrome.google.com/webstore/detail/githubbuddy/lbnnpglihcnokkjnmidginaihnojkfoo)($2.99/year)
- [Install a native app](https://github.com/Nicologies/GitHubBuddyHost/wiki/Installing-GitHubBuddyHost) to help launch the difftool

### Configure

#### GitHub Token

You will need to [create a personal access token](https://github.com/settings/tokens/new) with repo permission for this extension so it can retrieve the diff from github on your behalf.

#### Diff Tool Full Path

This is the full path to the difftool you want to use for example: `C:\Program Files (x86)\Beyond Compare 3\BCompare.exe`

By default we will pass two arguments to the difftool which are the file paths of the BASE and the HEAD. However, you can customise it with the $BASE and $HEAD placeholders if your difftool requires additional arguments.

#### Do Not Merge Label

This is a pull request label name, by default is `Do Not Merge`.

The extension will disable the Merge Button if the pull request is labelled as `Do Not Merge` (or the one you specified).


#### Text to Hyperlink

This is a feature to convert text to hyperlink. More specifically convert Issue to the link to an external system such as TFS, JIRA, Trac. Of course you could possibly use it to do something else as well.

See [here](https://github.com/Nicologies/GitHubBuddyHost/wiki/Text-to-Link-Configuration) for the instructions to configure it.

