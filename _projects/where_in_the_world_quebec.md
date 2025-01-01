---
layout: page
title: Where is the World for Quebec?
description:
img: assets/img/chloe_ucla_city_arrow.jpg
importance: 1
category: digital humanities
related_publications: false
---

We compiled, counted, characterized, and visualized claims to globality across texts published in 970 issues of indepedent magazines published in Montreal from the mid-1960s to the early 2000s, including _Revue Parti Pris_, _Cité Libre_, and _Québec-presse_. We prototyped the project on the 40 existing issues of _Revue Parti Pris_, iteratively experimenting with named entity recognition and topic modeling.

## Introduction

Where in the world for Quebec? From the 1960s to the 1990s, writers like Jacques Berque argued that Quebec was the most northern pole of Latin America. Pierre Vallières and others compared Quebec to Vietnam. Jean-Daniel Lafond imagined Quebec as being Caribbean. He and others imagined Montreal being an island in the Caribbean Sea and the counterpart to Martinique.

“It could very well be that this long-lost kinship is inscribed in an ancient miscegenation and in a clandestine creoleness that secretly unites the North and the South of the other America, of which only a few traces remain that only rare geographers can read and some signs that only poets still know how to decipher. So the old maps of the 17th century used to say: ‘South of Anticosti begins the Caribbean Sea…” (Jean-Daniel Lafond, _La Manière n_gre ou Aimé Césaire, chemin faisant_, 1993, pp 13, my translation)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/anticosti_carribean_sea.png" title="lafond" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Visualization produced at https://www.meridianoutpost.com/resources/etools/calculators/calculator-latitude-longitude-distance.php?
</div>

Anticosti is an island in the North Atlantic Ocean, and the Caribbean Sea is a large oceanic sea, also located in the basin of the North Atlantic, but at a very different lattitude (approx. -62.96 and 15.33)

## Questions

What can we say about "South of Anticosti begins the Caribbean Sea…"? How can we characterize this claim? Its a claim for a relationship between two places. Its also strictly untrue as a geographic claim in the most colloquial definition of geographic. How is the claim still productive if untrue?

We used named entity recognition (NER) and topic modeling to characterize the relationship established in the claims like "South of Anticosti begins the Caribbean Sea…" by scaling out to similar relationship between two places. In many cases, one of the two places if Quebec, or a place considered a part of Quebec, or places that contain Quebec, like North America. But the corpus also surfaced relationships between two places neither of which was Quebec.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/chloe_ucla_heatmap.png" title="heatmap" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Quebec heat map
</div>

## Corpus

We built a corpus of texts published in _Revue Parti Pris_, a Montreal-based leftist magazine that
published political and cultural commentary from 1963 to 1968. We used the scraper coded by
Quinn Dombrowski for Quebec’s National Library and Archives’ to aquire the complete print
production for Revue Parti Pris. We have just over 130,000 words accross 41 text files for the prototyped project. We have 

## Methods

Merve Tekgürler then helped me run huggingface for named entity recognition (NER), and topic modeling using BERTopic. As I understand it, both use transformer architecture. BERTopic functionally differs from Mallet, coded in java, in that clusters documents by content and then distinguishes content in each cluster. One key difference in the BERTopic model as opposed to Mallet is that it assumes each document belongs to a single topic, which is useful when looking to identify a phenomenon that isn’t necessarily going to be the dominant theme of the corpus. Ultimately, named entities broadly and location entities specifically are not the dominant themes of the corpus. If I had to guess, I would say topics relating to soveriegnty, imperlialism, and miliarization would surface as most frequent topics.

For NER, we extracted the plain text using ABBYY Fine Reader, then we split the collated texts into 500-word segments. We then ran the model on a loop for the 500-word segments. The model is a French NER called Flair. We saved all entities, including persons, organizations, locations, and miscellaneous, and then we saved only the locations separately. The model outputs a prediction accuracy, and we limited the locations to those that were 70 % predicted as locations and more. We then normalized these named location entities by turning everything to title case, and getting rid of those that were just one letter. After that we consolidated our named location entities as unique placenames, and summing the counts of those placenames. We then used a Wikidata plug-in for Google Drive in its French setting to associate placenames to Wikidata IDs, to get descriptions of the placenames, and to generate coordinates with an extension for Google Maps. We discarded the entities without Wikidata IDs.

Tekgürler took a second approach using the Wikidata IDs and a custom app script that they wrote after we detected irreguarities, which I think was the most time consuming part of this whole proof of concept. We ended up generating more coordinates in the second pass than the first. We then consoldated coordinates in the same rows that were matching. If there was no coordinate in the first pass, we defaulted to the second. If we had coordinates from both the first pass and the second, we still defaulted to the second. After all this, we eliminated all rows without coordinates, which resulted in 700 unique named location entities, down from 1,700 before the data clean up and consolidation.

Using Named Entity Recognition, we generated 700 unique named location entities, which included cities, countries, continents, village, plazas, parks, streets, rivers, train stations, universities, geological descriptors. I wanted to exclude Quebec and locations situated in Quebec. By logical extension, I also excluded named location entities that include Quebec, like Canada, North America, and Americas, but not South America and Latin America. I did not want to distract from the geographical range represented in the corpus, especially with scaled visualizations. For now, I am focusing on Quebec in relationship to other named location entities with the assumption that Quebec is the node for each of these discrete relationships. Hence, Where is the world for Quebec?

## Results and Analysis

## Conclusions



> Acknowledgements
> Thank you to Merve Tekgürler for making this project prototype possible in Fall 2023. Thank you to Quinn Dombrowski for initially coding the scraper in Winter 2023. Thank you to Em Ho for improving on Dombrowski's scraper in Summer 2024, and grow the corpus from 40 magazine issues to 970. My collaboration with Ho was made possible by the Stanford School of Humanities & Sciences through a Graduate Research Opportunity Grant.
