---
name: Calkulate
tools: [Alkalinity, Seawater chemistry, Python]
image: https://raw.githubusercontent.com/mvdh7/mvdh7.github.io/master/images/calkulate.png
description: Calkulate is a Python toolkit designed to transparently determine total alkalinity from titration pH data.
permalink: /projects/calkulate/
---

# Calkulate

**Calkulate is a Python toolkit designed to transparently determine total alkalinity from titration pH data.**

{% include button.html link="https://github.com/mvdh7/calkulate" text="GitHub" style="dark" %}
{% include button.html link="https://calkulate.readthedocs.io/en/latest/" text="Documentation" style="info" %}
{% include button.html link="https://raw.githubusercontent.com/mvdh7/mvdh7.github.io/master/citations/Humphreys2015Calkulate.bib" text="Citation" style="light" %}

Calkulate implements several different methods to determine alkalinity from titration data, including least-squares fitting and Gran plots. Calkulate can also determine the acid titrant molarity given measurements of reference materials with known alkalinity. All equilibrium constants and the solution composition can be varied independently, and every step in the determination process can be plotted and analysed separately.

v2.0 is in beta. The original v1.0 was written for MATLAB and only implements a half-Gran-plot method, but it is still [available on GitHub](https://github.com/mvdh7/calkulate/tree/1.0.2). A paper is in preparation describing v2.0, but for now the best citation to use is:

> Humphreys, M. P. (2015): Calculating seawater total alkalinity from open-cell titration data using a modified Gran plot technique, in *Measurements and Concepts in Marine Carbonate Chemistry*, pp. 25-44, PhD thesis, Ocean and Earth Science, University of Southampton, UK.

<!--![](https://mphumphreys.files.wordpress.com/2018/12/calkulate-f02.png)-->

<p class="text-center">
{% include button.html link="/projects" text="Back to projects" %}
</p>
