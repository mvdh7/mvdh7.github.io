---
name: MarChemSpec
tools: [Seawater chemistry]
image:
description: SCOR Working Group 145 - chemical speciation modelling of seawater for the 21<sup>st</sup> century.
permalink: /projects/marchemspec/
---

# MarChemSpec

## Pytzer

**Pytzer is a Python implementation of the Pitzer model for calculating physicochemical properties of complex aqueous solutions.**

{% include button.html link="https://github.com/mvdh7/pytzer" text="GitHub" style="dark" %}
{% include button.html link="https://pytzer.readthedocs.io/en/latest/" text="Documentation" style="info" %}
<!--{% include button.html link="https://raw.githubusercontent.com/mvdh7/mvdh7.github.io/master/citations/Humphreys2015Calkulate.bib" text="Citation" style="light" %}-->

Many properties of aqueous solutions, such as the chemical activities of the dissolved components, are given by different derivatives of a master equation for the excess Gibbs energy. Pytzer encodes the master equation, and then takes the novel approach of using automatic differentiation (as implemented by [Autograd](https://github.com/HIPS/autograd)) to evaluate each derivative of interest.

The core of the software and its API are stable, but Pytzer remains in beta for now as new sets of interaction coefficients to the model and tested. The present version also requires the concentration of each solute to be explicitly specified, including those that are involved in chemical equilibria. A future version will solve for these equilibria.

## SCOR Working Group 145

The MarChemSpec project is closely linked to [SCOR Working Group 145](http://marchemspec.org/).

<p class="text-center">
{% include button.html link="/projects" text="Back to projects" %}
</p>
