---
layout: post
title: "Visual Basic .NET and C# Outlining Extension"
description: "An extension to add additional outlining to Visual Basic .NET and C#"
category : Products 
tags : [Outlining, Utility, Products, Outliner, Extension, VSIX]
---

This extension provides additional outlining with syntax highlighted hint for Visual Basic .NET and C#; therefore we can collapse the blocks/scopes such as IF, ELSE, USING, WITH, WHILE, FOREACH and you name it.

Unlike the [C# Outline extension](https://visualstudiogallery.msdn.microsoft.com/9390e08c-d0aa-42f1-b3d2-5134aabf3b9a), which replaces the outlining provided by Visual Studio, this extension keeps it untouched, as a result the syntax highlighting remains unchanged.


<!--more-->

# Installing

You can either search `Visual Basic and C# Outliner` and install it via the Visual Studio Extension Manager or download it manually from [here](https://visualstudiogallery.msdn.microsoft.com/699ecb9d-bd8f-4f6b-b290-05c42407b43e)

Due to the dependencies of the Roslyn dlls, it only supports Visual Studio 2015, and there is no plan to port it back to ealier version of Visual Studio.

# Features

## CSharp

As you can see from the below image, there are couple of additional outliners added to the editor, so we can collapse them.

![C# collapsible scopes](/images/VSOutliner/CSharpCollapsibleScopes.png)

Below is how it looks after collapsed

![C# Collapsed](/images/VSOutliner/CSharpCollapsed.png)

Additionally, it will show the collapsed content with syntax highlighted when mouse is hovering on:

![C# Outlining Hint](/images/VSOutliner/CSharpOutliningHint.png)

## Visual Basic .NET

In addition to C#, this extension also supports Visual Basic .NET:

![Visual Basic collapsible scopes](/images/VSOutliner/VBCollapsibleScopes.png)
