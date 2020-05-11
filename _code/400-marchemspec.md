---
name: Physical chemistry of seawater
tools: [Seawater chemistry, Python]
image:
description: Modelling chemical activities and equilibria in seawater with Pytzer, a Python implementation of the Pitzer model.
permalink: /code/pytzer/
---

# **Physical chemistry of seawater**

## **Automatic differentiation for physical chemistry**

**Pytzer** is a Python implementation of the Pitzer model for calculating physicochemical properties of complex aqueous solutions using automatic differentiation.

{% include elements/button.html link="https://github.com/mvdh7/pytzer" text="GitHub" style="dark" size="lg" %}
{% include elements/button.html link="https://pytzer.readthedocs.io/en/latest/" text="Documentation" style="info" size="lg "%}

Many properties of aqueous solutions, such as the chemical activities of the dissolved components, are given by different derivatives of a master equation for the excess Gibbs energy.  Pytzer encodes the master equation, and then takes the novel approach of using automatic differentiation (as implemented by [Autograd](https://github.com/HIPS/autograd)) to evaluate each derivative of interest.

The core of the software and its API are stable, but Pytzer remains in beta for now as new sets of interaction coefficients to the model and tested.  The present version also requires the concentration of each solute to be explicitly specified, including those that are involved in chemical equilibria.  A future version will solve for these equilibria.

## **SCOR Working Group 145 and the MarChemSpec project**

Pytzer started out its life in the **MarChemSpec** project.

MarChemSpec is a research project jointly funded by the Natural Environment Research Council (NERC, UK) and National Science Foundation (NSF, USA), with collaborators from the University of East Anglia, Scripps Institution of Oceanography, and Woods Hole Oceanographic Institution. Other project partners include the national metrological institutes of the USA (NIST), France (LNE), Germany (PTB) and Japan (NMIJ), and [SCOR Working Group 145](http://marchemspec.org/).

I worked on MarChemSpec as a postdoc for 2 years at the University of East Anglia.  The results of my work were presented at the Ocean Sciences Meeting 2020 in San Diego.  You can find my abstract on the [meeting website](https://agu.confex.com/agu/osm20/meetingapp.cgi/Paper/642945).

<p class="text-center">
{% include elements/button.html link="/code" text="Back to code" size="lg" %}
</p>
