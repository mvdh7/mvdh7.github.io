---
title: Making MATLAB figures 2 - gcf and print
tags: [Poetry]
style: fill
color: light
description:
---

The appearance of a new MATLAB figure never fails to disappoint.

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/dlwdnrqwaaawavp.jpg" caption="Sadly, a raw MATLAB figure would never reach these dizzying heights." %}

Fortunately, we can adjust totally reinvent the figure using a few choice commands, which I will go through over the course of a few posts.

## Get current figure

We'll begin today with `gcf`, which stands for 'get current figure'. Once you have created a figure ([ideally, as described here](/articles/making-matlab-figures-1-basic-workflow)), you can use `gcf` to modify its appearance.
