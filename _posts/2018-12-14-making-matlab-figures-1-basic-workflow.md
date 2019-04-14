---
title: Making MATLAB figures 1 - basic workflow
tags: [MATLAB, Dataviz, Workflow]
style: fill
color: light
description: The first step in a simple workflow for efficiently designing publication-quality figures in MATLAB.
---

Having produced most of my scientific figures primarily with MATLAB since early on in my PhD (6 years and counting), I’ve now developed quite an efficient system, which I’d like to share. This will be the first in a series of blog posts, each looking at a different aspect of MATLAB figuresmithing.

If you have anything to add or alternative suggestions I'd love to hear them! This is just the way I've settled on doing things; it works well for me, but I'm always open to making it better.

First we'll just look at the overall workflow of how to efficiently create and work on a new figure, without worrying about the details of what's in the figure or making it look good.

## EVERY time you make a new figure...

Go to the Command Window and type `edit`, to open a new Editor window. Make a new script file for each figure! And, keep these separate from your scripts that actually load and manipulate the data. Write the following, at the top of the Editor script:

```matlab
figure(1); clf
```

Save (Ctrl + S) and execute the script (Ctrl + Enter). A new figure window should pop up. Dock the window by clicking the black arrow near the top right corner:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2018/12/dockfig.png" caption="Click the black arrow at the top right of the pop-up figure window to dock it." %}

Your figure is now docked in the main MATLAB window! You can move things around by clicking and dragging the different sub-windows' title bars. I like to have mine set up as below:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2018/12/matlabdock.png" caption="Figure docked at the bottom right." %}

Now, we construct the figure by adding to the script in the Editor window. I will go into this side of things in loads more detail in other posts. For now let's just put in some nonsense to show the benefit of this docking approach. For example, add some scattered data points to your code so it looks like this:

```matlab
figure(1); clf
scatter(1:5, randn(5, 1))
```

Now, when we execute the code in the editor (with Ctrl + Enter), the docked figure stays in its place, and is refreshed with the new code added in. This approach means that there is no need to jump backwards and forwards between windows, and an updated figure is re-drawn each time in the same place. It makes for a really convenient working environment to then incrementally build up the figure by adding to the Editor script.

## The MATLAB magic

The key part is the opening command, `figure(X); clf`. There's not much to it, but let's break it down:

`figure(X)`, where `X` can be any positive integer, either:

  * Creates a new figure window with the number `X`, if it's not already open;
  * Selects the figure window with the number `X`, if it is already open.

So by specifying a value for `X`, we make sure that every time we re-run the Editor script after making updates (with Ctrl + Enter), the updates are applied to the same docked figure window.

`clf` clears the contents of the active figure window. This means that each time you re-run the Editor script it draws the figure from a fresh start, instead of just adding it on top of whatever's already in the figure window.

## In summary

  * Make a new Editor script for each figure;
  * Start every figure script with `figure(X); clf` (where `X` can be any positive integer);
  * Dock the figure window and incrementally build the figure by adding to the Editor script.
