---
# File metadata may be provided as frontmatter YAML
title: Bayesian Parameter Estimation
subtitle: Exploring the Bayesic principles in a few canonical contexts
description: Analysis of the seismic earthquake data during the eruption
date: 2023-09-09
tags:
  - volcano
  - seismicity
  - la-palma
thumbnail: images/la-palma-eruption-2022-paper.png
---

+++ {"part":"abstract"}

% The article should include an abstract block at the beginning. The block is delimited by `+++` before and after, and you must specify `"part": "abstract"` as JSON metadata on the block opener. This metadata is required for recognizing the content of this cell as the abstract.
% The abstract should begin with a short description of the problem addressed, briefly describe the new data or analyses, then briefly state the main conclusion(s) and how they are supported, and address any uncertainty.

abstract lorem ipsum etc.

+++

# Introduction
If you needed to estimate parameters – that is to say, fit a model – the first mathematical tool you would likely reach for would be linear regression. In this article, we'll explore an alternative to linear regression called Bayesian Parameter Estimation and consider in what circumstances it might be preferable. 

# Background: Bayes' Theorem

Bayes' Theorem is a simple statement about conditional probabilities:
```{math}
:label: bayes

P(H|E) = \frac{P(E|H)P(H)}{P(E)}
```
In this notation $H$ is a hypothesis and $E$ is some evidence we have observed. So Bayes' theorem tells us that we can compute the probability of some hypothesis being true given that we have observed a piece of evidence if we know:
* the probability of observing said evidence in a world where the hypothesis is true, 
* the probability of our hypothesis being true, and
* the probability of observing the evidence.

It can be "derived" by considering that we can get the area of region AB below both by considering it either as the fraction of A that AB occupies multiplied by the total area of A *or* as the fraction of B that AB occupies multiplied by the total area of B. (The "Universe" is included to remind us that for A and B to represent probabilities, they have to be normalized!). For a more detailed version of this explanation, check out Oscar Bonilla's [blog post on "Visualizing Bayes' Theorem](https://oscarbonilla.com/2009/05/visualizing-bayes-theorem/).

```{figure} images/venn-last.png
:name: bayes-venn
:align: center
:width: 70%

Visualizing the "derivation" of Bayes' theorem, from {cite:t}`bonilla`.
```

# Eruption History

A number of eruptions were recorded since the colonization of the islands by Europeans in the late 1400s, these are summarized in [Table %s](#history).

> Simple tables may be created using the [list-table directive](https://mystmd.org/guide/tables). Similar to figures, tables may be referenced in the text by their `name`. The caption for this table is the first line of the directive.

```{list-table} Recent historic eruptions on La Palma
:header-rows: 1
:name: history
* - Name
  - Year
* - Current
  - 2021
* - Teneguía
  - 1971
* - Nambroque
  - 1949
* - El Charco
  - 1712
* - Volcán San Antonio
  - 1677
* - Volcán San Martin
  - 1646
* - Tajuya near El Paso
  - 1585
* - Montaña Quemada
  - 1492
```

This equates to an eruption on average every 79 years up until the 1971 event. The probability of a future eruption can be modeled by a Poisson distribution [](#poisson).

> Numbered equations may be defined using the [math directive or in line](https://mystmd.org/guide/math). Equations defined with the math directive may be reference in the text by label.

```{math}
:label: poisson

p(x)=\frac{e^{-\lambda} \lambda^{x}}{x !}
```

Where $\lambda$ is the number of eruptions per year, $\lambda=\frac{1}{79}$ in this case. The probability of a future eruption in the next $t$ years can be calculated by:

```{math}
:label: probability

p_e = 1-\mathrm{e}^{-t \lambda}
```

So following the 1971 eruption the probability of an eruption in the following 50 years — the period ending this year — was 0.469. After the event, the number of eruptions per year moves to $\lambda=\frac{1}{75}$ and the probability of a further eruption within the next 50 years (2022-2071) rises to 0.487 and in the next 100 years, this rises again to 0.736.

## Magma Reservoirs

> You may [add citations two ways](https://mystmd.org/guide/citations). First, you may simply insert a markdown link to a DOI like so: [](10.1093/nar/22.22.4673). No additional bibliographic information is required for this approach; the reference will be looked up by DOI and added implicitly to the references. Alternatively, you may provide the bibliography directly as `references.bib` BibTeX file, then embed the citation by BibTeX key in your text using the `cite:p` or `cite:t` for parenthetical or textual citations, respectively. The following paragraph provides an example of this. A single paper may combine both DOI and BibTeX citations.

Studies of the magma systems feeding the volcano, such as {cite:p}`marrero2019` has proposed that there are two main magma reservoirs feeding the Cumbre Vieja volcano; one in the mantle (30-40km depth) which charges and in turn feeds a shallower crustal reservoir (10-20km depth).

```{figure} images/reservoirs.png
:name: reservoirs
:align: center
:width: 100%

Proposed model from {cite:t}`marrero2019`.
```

In this paper, we look at recent seismicity data to see if we can see evidence of such a system action, see [Figure %s](#reservoirs).

# Dataset

> All data used in the notebook should be present in the `data/` folder so notebooks may be executed in place with no additional input.

The earthquake dataset used in our analysis was generated from the [IGN web portal](https://www.ign.es/web/resources/volcanologia/tproximos/canarias.html) this is public data released under a permissive license. Data recorded using the network of [Seismic Monitoring Stations](#stations) on the island. A web scraping script was developed to pull data into a machine-readable form for analysis. That code tool [is available on GitHub](https://github.com/stevejpurves/ign-earthquake-data) along with a copy of recently updated data.

# Results

The dataset was loaded into a Jupyter notebook [visualization](./notebooks/visualization-figure-creation-seaborn.ipynb) and filtered down to La Palma events only. This results in 5465 data points which we then visualized to understand their distributions spatially, by depth, by magnitude and in time.

```{figure} #eq-timeline
:name: timeline

Earthquake data over time (n=5465) to understand their distributions spatially, by depth, by magnitude and in time.
This figure uses cell output from the [visualization notebook](./notebooks/visualization-figure-creation-seaborn.ipynb). The first line of the [cell](#eq-timeline) is `#| label: eq-timeline`. Referencing that label pulls in the output of the cell as a figure.
```

From our analysis in [Figure %s](#timeline), we can see 3 different systems in play.

Firstly, the shallow earthquake swarm leading up to the eruption on 19th September, related to significant surface deformation and shallow magma intrusion.

After the eruption, continuous shallow seismicity started at 10-15km corresponding to magma movement in the crustal reservoir.

Subsequently, high magnitude events begin occurring at 30-40km depths corresponding to changes in the mantle reservoir. These are also continuous but occur with a lower frequency than in the crustal reservoir.

# Conclusions

From the analysis of the earthquake data collected and published by IGN for the period of 11 September through to 9 November 2021. Visualization of the earthquake events at different depths appears to confirm the presence of both mantle and crustal reservoirs as proposed by {cite:t}`marrero2019`.

+++ {"part":"availability"}

> Data availability statement should be specified in a separate block with metadata `"part": "availability"`, similar to the abstract.
>
> AGU requires an Availability Statement for the underlying data needed to understand, evaluate, and build upon the reported research at the time of peer review and publication.

A web scraping script was developed to pull data into a machine-readable form for analysis. That code tool [is available on GitHub](https://github.com/stevejpurves/ign-earthquake-data) along with a copy of recently updated data.
