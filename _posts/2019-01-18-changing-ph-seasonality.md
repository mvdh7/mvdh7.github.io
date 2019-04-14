---
title: Changing pH seasonality
tags: [Seawater chemistry, Ocean acidification]
style: fill
color: danger
description:
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
MathJax.Ajax.config.path["mhchem"] =
  "https://cdnjs.cloudflare.com/ajax/libs/mathjax-mhchem/3.3.2";
MathJax.Hub.Config({TeX: {extensions: ["[mhchem]/mhchem.js"]}});
</script><script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML' async></script>

This post is an extension of a Twitter thread I wrote November, after reading Kwiatkowski and Orr's paper on [Diverging seasonal extremes for ocean acidification during the twenty-first century](https://www.nature.com/articles/s41558-017-0054-0):

<blockquote class="twitter-tweet"><p dir="ltr" lang="en">Totally counter-intuitive but enormously important point for ocean acidification research raised in this <a href="https://twitter.com/NatureClimate?ref_src=twsrc%5Etfw">@NatureClimate</a> study by Kwiatkowski &amp; Orr <a href="https://t.co/LwF7RxQGiN">https://t.co/LwF7RxQGiN</a> [1/n]</p>— Matthew Humphreys (@matthew_vdh) <a href="https://twitter.com/matthew_vdh/status/1067100603019653122?ref_src=twsrc%5Etfw">November 26, 2018</a></blockquote>

## Seawater pH and its seasonal cycles

About a quarter of humanity's carbon dioxide emissions each year dissolve into the ocean. This causes some chemical reactions in the water that increase the concentration of hydrogen ions, [$\ce{H+}$]. We tend to record this concentration using pH, which is:

$$pH = -log_{10} [\ce{H+}]$$

The maths of this relationship means that as [$\ce{H+}$] goes up, pH goes down.

As well as the long-term trend, pH in the ocean changes seasonally. These seasonal changes happen because of warming and cooling, and also because of how the life cycles of marine plants and animals affect the chemistry of the surrounding seawater.

Not only is the average pH over the course of each year changing, but in some places, so too is the strength of its seasonal cycles. Stronger seasonal cycles mean a bigger range of pH values that organisms have to deal with, which might be problematic.

The most exciting point (for me!) raised by Kwiatkowski and Orr was that **just because you have a change in the strength of the pH seasonal cycle, it doesn't mean that the [$\ce{H+}$] cycle will respond in the same way.**

## Seasonality at constant average pH

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/phvar_initial-4.gif" caption="Here you can see the logarithmic relationship between pH and hydrogen ion concentration, [$\ce{H+}$], and how they might change together through the seasons at some imaginary point in today's ocean." %}

In the example above, we have a seasonal cycle in pH of 0.4 units, about a mean pH of 8.2, which corresponds to a seasonal cycle in [$\ce{H+}$] of about 0.006 μmol·kg<sup>−1</sup>.

If we change the size of the seasonal cycle of pH while staying at the same average pH value, then as you would intuitively expect, the seasonal cycle in [$\ce{H+}$] responds in the same way:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/phvar_same_baseline-1.gif" caption="A greater seasonal cycle in pH leads to a greater seasonal cycle in [$\ce{H+}$] (and vice versa) if the average pH value doesn't change." %}

## Changing the average pH

Where things get interesting is when you couple the change in seasonality with a change in the mean pH value. First, let's decrease the average pH down to 7.6, but keep the pH seasonal cycle the same:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/phvar_constant_ph-1.gif" caption="Average pH has gone down, and the seasonal cycle in pH has not changed - but the seasonal cycle in [$\ce{H+}$] is four times greater!" %}

Despite no change in the pH seasonal cycle, we have four times more variability in [$\ce{H+}$] at the lower average pH.

Similarly, if we hold the [$\ce{H+}$] cycle constant at that lower pH, we find that this makes pH switch to a seasonal cycle four times smaller:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/phvar_constant_hconc.gif" caption="This time the seasonal cycle in [$\ce{H+}$] is unchanged at the lower pH value, so the seasonal cycle in pH is four times smaller to compensate." %}

The actual value of the ratio between the seasonal cycles in pH and [$\ce{H+}$] is constant for any given mean pH, and is set by Eq. (1) given by Kwiatkowski and Orr.

## Who cares?

This matters because it is probably [$\ce{H+}$], rather than pH, that directly affects marine species and ecosystems. But when monitoring ocean acidification, it's easy to slip into simply looking at pH, because that's how you record the measurements. As I put it previously:

<blockquote class="twitter-tweet"><p dir="ltr" lang="en">... meanwhile, the seasonal variability in the total hydrogen ion concentration - plausibly more important to the organisms in the water than pH is - could be undergoing the opposite change, thus turning your conclusions about biological impact on their head [8/n]</p>— Matthew Humphreys (@matthew_vdh) <a href="https://twitter.com/matthew_vdh/status/1067100612704243712?ref_src=twsrc%5Etfw">November 26, 2018</a></blockquote>

## In summary

  * The fact that the average ocean pH is changing means that seasonal cycles in pH and [$\ce{H+}$] may change differently from each other;
  * If you're studying these changes, look at both variables!

I'll write up a post demonstrating the techniques used to make the figures in this post at a later date, but if you want to have a look in advance, the code is all [available from my GitHub](https://github.com/mvdh7/mvdh-xyz).
