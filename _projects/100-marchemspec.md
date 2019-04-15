---
name: Marine chemical speciation modelling
tools: [Seawater chemistry, Python, Julia]
image:
description: Deploying automatic differentiation for chemical speciation modelling of seawater, and understanding the underlying uncertainties.
permalink: /research/marchemspec/
---

# **MarChemSpec**

## Automatic differentiation for physical chemistry

**Pytzer is a Python implementation of the Pitzer model for calculating physicochemical properties of complex aqueous solutions using automatic differentiation.**

{% include button.html link="https://github.com/mvdh7/pytzer" text="GitHub" style="dark" %}
{% include button.html link="https://pytzer.readthedocs.io/en/latest/" text="Documentation" style="info" %}

Many properties of aqueous solutions, such as the chemical activities of the dissolved components, are given by different derivatives of a master equation for the excess Gibbs energy. Pytzer encodes the master equation, and then takes the novel approach of using automatic differentiation (as implemented by [Autograd](https://github.com/HIPS/autograd)) to evaluate each derivative of interest.

The core of the software and its API are stable, but Pytzer remains in beta for now as new sets of interaction coefficients to the model and tested. The present version also requires the concentration of each solute to be explicitly specified, including those that are involved in chemical equilibria. A future version will solve for these equilibria.

## SCOR Working Group 145

MarChemSpec is jointly funded by the Natural Environment Research Council (NERC, UK) and National Science Foundation (NSF, USA), with collaborators from the University of East Anglia, Scripps Institution of Oceanography, and Woods Hole Oceanographic Institution. Other project partners include the national metrological institutes of the USA (NIST), France (LNE), Germany (PTB) and Japan (NMIJ), and [SCOR Working Group 145](http://marchemspec.org/).

<p class="text-center">
{% include button.html link="/research" text="Back to research" %}
</p>
