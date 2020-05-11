---
title: The marine motivation for Pitzer models
tags: [Seawater chemistry, MarChemSpec]
style: outline
color: secondary
description: How do tools like the Pitzer model help us to understand chemical equilibria in seawater?
comments: false
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
MathJax.Ajax.config.path["mhchem"] =
  "https://cdnjs.cloudflare.com/ajax/libs/mathjax-mhchem/3.3.2";
MathJax.Hub.Config({TeX: {extensions: ["[mhchem]/mhchem.js"]}});
</script><script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML' async></script>

For my postdoctoral position at the University of East Anglia, I am working on the Pitzer model, which calculates the chemical activity of the atoms and molecules dissolved in complex solutions like seawater. This work is part of the [MarChemSpec project](http://marchemspec.org/), jointly funded by NERC (UK) and NSF (USA).

Here, I attempt to explain why we need the Pitzer model, and what it does – *how* it does it, I'll leave for another time.

## What happens in seawater?

On an atomic scale, seawater is a very busy place. Many of its ingredient chemicals constantly crash into each other and react, switching to and fro between different forms. These reactions happen in both directions at the same time: just as a hydroxide ion ($\ce{OH-}$) and a proton ($\ce{H+}$) meet and merge into a water molecule ($\ce{H2O}$), a neighbouring $\ce{H2O}$ will split itself in two.

The rates of these opposite processes quickly reach a balance called **dynamic equilibrium**, from which point the overall amount of each chemical in the solution does not change, despite the ongoing frenetic reactions.

{% include elements/figure.html image="https://mphumphreys.files.wordpress.com/2019/01/65587_10151339029213264_979264413_n.jpg" caption="Some rather exciting seawater. Unfortunately, part of the view is ruined by an errant penguin-laden iceberg." %}

## Who cares about dynamic equilibrium?

Consider carbon dioxide ($\ce{CO2}$). It dissolves into seawater and reacts. At equilibrium, only about one in a hundred of the $\ce{CO2}$ molecules that entered the water still remain. About ten of the hundred will have changed into carbonate ions ($\ce{CO3^2-}$), and the rest will be bicarbonate ($\ce{HCO3-}$). If we wanted to work out, for example, how stable the calcite ($\ce{CaCO3}$) that oysters build their shells out of will be, we need to know the $\ce{CO3^2-}$ concentration alone. This isn't widely measured, but we can calculate it from the total amount of all three forms of dissolved $\ce{CO2}$ – which is easier to establish – and a second variable like the seawater pH (a measure of the amount of $\ce{H+}$). To perform this calculation, we need a value for the **stoichiometric equilibrium constant** for the reaction.

A stoichiometric equilibrium constant tells us how much of each chemical there will be when a system is in dynamic equilibrium. The unpleasant word 'stoichiometric' just means that we are dealing in concentrations. This constant derives from a **thermodynamic** equilibrium constant, which is different for each different reaction. The thermodynamic constant is affected by temperature alone, whereas the **composition** of the solution –  the amount and type of each dissolved component – also influences the stoichiometric version.

## From thermodynamic to stoichiometric

How do we get an equilibrium constant from thermodynamic to stoichiometric? We calculate the conversion as a function of the seawater composition.

In the open ocean, away from the complications of rivers, ice, and the restless Earth, the composition of seawater has internal consistency. Concentrations of the major ions are mostly just increased by evaporation or decreased by rainfall, so the ratios between themselves and with overall salinity stay fixed. This means that we can make a straightforward, empirical equation that just uses salinity to convert between the different equilibrium constants.

{% include elements/figure.html image="https://mphumphreys.files.wordpress.com/2019/01/stoichiometric-equilibrium-1.png" caption="Schematic of two approaches to getting a stoichiometric equilibrium constant ($K^* $). Top: from salinity, assuming constant ionic ratios. Bottom: from a Pitzer model, taking into account the solution composition, and working in more complex environments." %}

However, when we move into more complex environments, these ratios are not so fixed. The simple salinity equations no longer work. We need our calculations to be more sophisticated – enter the **Pitzer model**.

## The Pitzer model

So, from the marine chemist's perspective, the Pitzer model is just a way to calculate how to convert thermodynamic equilibrium constants, which are relatively well-known and actually constant, into a stoichiometric form, which can actually be used to work out what's going on in your solution.

How does it do that? Best left for another time. Suffice it to say, a lot of Greek letters are involved.

{% include elements/figure.html image="https://mphumphreys.files.wordpress.com/2019/01/win_20190125_14_29_49_pro-2.jpg" caption="Yum." %}

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://mvdh.xyz/blog/the-marine-motivation-for-pitzer-models';  // Replace PAGE_URL with your page's canonical URL variable
// this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://mvdh7.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
