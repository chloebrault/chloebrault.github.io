---
layout: page
title: Where is the World for Quebec?
description:
img: assets/img/chloe_ucla_city_arrow.jpg
importance: 1
category: digital humanities
related_publications: false
---

We compiled, counted, characterized, and visualized claims to globality across texts published in 970 issues of independent magazines published in Montreal from the mid-1960s to the early 2000s, including _Revue Parti Pris_, _Cité Libre_, and _Québec-presse_. We prototyped the project on the 40 existing issues of _Revue Parti Pris_, iteratively experimenting with named entity recognition and topic modeling.

## Introduction

Where is the world for Quebec? From the 1960s to the 1990s, writers like Jacques Berque argued that Quebec was the most northern pole of Latin America. Pierre Vallières and others compared Quebec to Vietnam. Jean-Daniel Lafond imagined Quebec as being Caribbean. He and others imagined Montreal being an island in the Caribbean Sea and the counterpart to Martinique.

“It could very well be that this long-lost kinship is inscribed in an ancient miscegenation and in a clandestine creoleness that secretly unites the North and the South of the other America, of which only a few traces remain that only rare geographers can read and some signs that only poets still know how to decipher. So the old maps of the 17th century used to say: ‘South of Anticosti begins the Caribbean Sea…”[^fn1].

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/anticosti_carribean_sea.png" title="lafond" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Visualization produced with the Latitude/Longitude Distance Calculator by Meridian Outpost.
</div>

Anticosti is an island in the North Atlantic Ocean, and the Caribbean Sea is a large oceanic sea, also located in the basin of the North Atlantic, but at a very different latitude. The geographical center of the Anticosti Island is at 49°32′N and 63°14′W. On the other hand, the northernmost point of the Carribean Sea, the Windward Channel between the islands of Cuba and Hispaniola, has its center located at 20°N and 74°W.

## Questions

What can we say about “South of Anticosti begins the Caribbean Sea…”? How can we characterize this claim? It is a claim for a relationship between two places. It is also strictly untrue as a geographic claim in the most colloquial definition of geographic. How is the claim still productive if untrue?

We used named entity recognition (NER) and topic modeling to characterize the relationship established in the claims like "South of Anticosti begins the Caribbean Sea…" by scaling to similar relationships between two places. In many cases, one of the two places is Quebec, or a place considered a part of Quebec, or places that contain Quebec, like North America. But the corpus also surfaced relationships between two places, neither of which was Quebec.

## Corpus

We built a corpus of texts published in _Revue Parti Pris_, a Montreal-based leftist magazine that published political and cultural commentary from 1963 to 1968. We used the scraper coded by Quinn Dombrowski for Quebec’s National Library and Archives’ to acquire the complete print production for Revue Parti Pris. We had about 130,000 words across 41 text files for the prototyped project. Later, we expanded the corpus to about 26 million words across 970 text files. This text acquisition was made possible by Em Ho, who [adapted](https://colab.research.google.com/drive/1nrfxZSo5kfcNdCSymh938AoCTa5y8VDU) Dombrowski’s scraper.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/BANQ_scraper.png" title="BANQ_scraper" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Desktop view of Quebec's National Library and Archives online holdings, queried for independent magazines published between the mid 1960s and the early 2000s. 
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/BANQ_scraper_item1.png" title="BANQ_scraper_item1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Desktop view of first item of the query.
</div>

## Methods

Merve Tekgürler then helped me run [Flair](https://huggingface.co/flair/ner-french), a French model for named entity recognition (NER), and topic modeling using BERTopic.

For NER, we extracted the plain text using ABBYY FineReader, then we split the collated texts into 500-word segments. [We then ran the model](https://colab.research.google.com/drive/15hYe3wimKr5bmZndOOT7rQwRnH2jAs-q) for the 500-word segments. We saved all entities, including persons, organizations, locations, and miscellaneous, and then we saved only the locations separately. Flair outputs a prediction accuracy, and we limited the locations to those that were 70 % predicted as locations and more. We then normalized these named location entities by turning everything to title case, and getting rid of those that were just one letter. After that we consolidated our named location entities as unique place names, and summed the counts of those place names. We then used a Wikidata plug-in for Google Drive in its French setting to associate placenames to Wikidata IDs, to get descriptions of the place names, and to generate coordinates with an extension for Google Maps. We discarded the entities without Wikidata IDs.

Tekgürler took a second approach using the Wikidata IDs and a custom app script that they wrote after we detected irregularities, which was the most time consuming part of this whole proof of concept. We ended up generating more coordinates in the second pass than the first. We then consolidated coordinates in the same rows that were matching. If there was no coordinate in the first pass, we defaulted to the second. If we had coordinates from both the first pass and the second, we still defaulted to the second. After all this, we eliminated all rows without coordinates, which resulted in 700 unique named location entities, down from 1,700 before the data clean up and consolidation.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NER&WikiDataID.png" title="NER&WikiDataID" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Data generated from scraped corpus, NER, and Wikidata pipeline
</div>

Using named entity recognition, we generated 700 unique named location entities, which included cities, countries, continents, villages, plazas, parks, streets, rivers, train stations, universities, and geological descriptors. We wanted to exclude Quebec and locations situated in Quebec. By logical extension, we also excluded named location entities that include Quebec, like Canada, North America, and Americas, but not South America and Latin America. We did not want to distract from the geographical range represented in the corpus, especially with scaled visualizations. For now, we are focusing on Quebec in relation to other named location entities with the assumption that Quebec is the node for each of these discrete relationships. Hence, Where is the world for Quebec?

For topic modeling, we had already extracted the plain text using ABBY FineReader, but then we added paragraph breaks, and adjusted for false paragraph breaks using a rule-based code that joined any lower case word to the previous paragraph. We ended up with 22,000 segments and then ran BERTopic on these, which produced 282 topics. We then extracted two types of information from those topics. The first type of information was the most frequently associated words in each topic, retaining the probability of those topics, and generating both word clouds and a CVS. The second type of information was association of topic to individual paragraphs, after we mapped to each paragraph the topic that is most associated with that paragraph, generating the inverse probabilities. Last, we filtered out the paragraphs with topic minus one in the CSV, and ended up with about 700 paragraphs that have meaningful topics. I did a lot of cleaning for the NER, specifically to exclude mentions of Quebec, but this was not warranted for the topic modeling.

Of 282 topics, we found relationships between unique place names visualized by the model. Some of these unique place names were in relation to Quebec, other unique place names were related to more unique place names. We also found unique place names in relationship with and topic clusters that had nothing to do with location: like war, independence, miliary, etc., which confirms by guess that the most frequent topics in this corpus would relate to sovereignty, imperialism, and militarization even without the specific most frequent topics output. We chose to highlight word clouds that had two or more place names. When we look at these word clouds, we are wondering if these place names could be simple placeholders that could be swapped out for others.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wordcloud1.png" title="wordcloud1" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Topic model generated world cloud representing several Latin American countries. 
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wordcloud2.png" title="wordcloud2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     Topic model generated world cloud representing several Middle Eastern countries.
</div>

## Results and Analysis

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/chloe_ucla_country_more_than_20.png" title="chloe_ucla_country_more_than_20" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Map made my Merve Tekgürler using Arc GIS.
</div>

In this first map, we see unique place names visualized by dots. I have to say that the geographic range surprised me. I actually was expecting more representation in Asia and Africa, probably because those placenames impressed me the most when I was reading without computational tools.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/chloe_ucla_heatmap.png" title="heatmap" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Map made my Merve Tekgürler using Arc GIS. 
</div>

In this second map, we see scaled place names, with the highest frequency of place names represented by yellow, then orange, red, and the lowest frequencies represented by pink and purple. The country place names that ranked highest in frequency were the United States, France, Canada, Cuba, and Vietnam. The city place names that ranked the highest were Ottawa and Paris.

## Analysis

I initially called this project Islands that Repeat Themselves. The title was initially a nod to precisely the fact that I was not unconvinced that place names were not just placeholders. In other words, I guessed that place names outside of Quebec were placeholders for one another and for Quebec itself. I was not only thinking of Lafond’s fabulation in titling the paper, but of Antonio Benítez-Rojo’s 1989 book, The Repeating Island: The Caribbean and the Postmodern Perspective. Benítez-Rojo argues that the Caribbean is best understood as an iterative meta-archipelago, which constantly changes and reconfigures space in a way that defies linear time and geographic categorization. In Benítez-Rojo’s words, “an island that ‘repeats’ itself, unfolding and bifurcating “South of Anticosti begins the Caribbean Sea…” is an “unwarranted [simplification] until it reaches all the seas and lands of the earth, […] inspires multidisciplinary maps of unexpected designs.”[^fn2] We can certainly call the map that Lafond describes “multidisciplinary” and “of unexpected design.”

However, “South of Anticosti begins the Caribbean Sea…” is more than “multidisciplinary” and “of unexpected design.” I found Martin Lewis and Karen Wigen’s concept theorization of “meta-geography” useful for understanding how this is productively untrue. Lewis and Wigen define meta-geography as the large-scale “set of spatial structures through which people order their knowledge of the world.”[^fn3] That “ideological power” is behind “unwarranted simplifications of global spatial patterns” (xiii).

To say that “South of Anticosti begins the Caribbean Sea…” is an “unwarranted [simplification] of global spatial patterns”. Using NER outputs, I argue in the conclusion that the ideological power inscribed in the relationship between Anticosti and the Caribbean Sea is colonialism.

## Conclusion

What I could have told you before NER and topic modeling is that Lafond made a documentary film about the Martinican poet Aimé Césaire. The book that I am citing describes the film’s content and how it was made. The documentary’s conceit is that Quebecois poet Paul Chamberland travels to Martinique to interview Césaire. He observes similarities between Caribbean and Quebec culture and poetics. He asks Césaire to speak about what Quebec means to him. I could have also told you that in order to establish sameness between Caribbean and Quebec, Lafond relied on spatialized proof external to his own analysis, that is “few traces remain that only rare geographers can read and some signs that only poets still know how to decipher.” [^fn4] Shared origins in space and time—esoteric as they may be—beget shared cultural lineage between the Caribbean and Quebec. Shared cultural lineage begets similarities, proximity, intimacy, and fraternity.

What can I say about ‘South of Anticosti begins the Caribbean Sea…’ after NER and topic modeling?

With NER, I noted historical place names like Lower Canada, which was the British colonial title for present-day southern Quebec, Labrador, and Newfoundland, and Upper Canada, which was the British colonial title for present-day southern Ontario. I manually eliminated Lower Canada for the Quebec coverage, but kept Upper Canada for its lack of Quebec coverage. Other historical place names include Upper Volta, present-day Burkina Faso, which was a colony of French West Africa and Afrique Noire, literally Black Africa, the French colonial designation for sub-Saharan Africa. With historical place names, I found that the writers at _Revue Parti Pris_ imagined Quebec not only in space as I had initially observed, but also in space in relation to time. Lower-Canada is only coherent in space in specific historical context, and the same goes for Afrique Noire. Such historical place names bring colonialism to the fore. With colonialism as a filter for the larger corpus, I could characterize ‘South of Anticosti begins the Caribbean Sea…’ as not only untrue in the most colloquial definition of a geographic claim, but untrue and colonial. At some point I started thinking of ‘South of Anticosti begins the Caribbean Sea…’ as a colonial hallucination. I also found things in this proof of concept that I otherwise would not have necessarily looked for, starting with a new reference to Anticosti Island. But Anticosti Island did come up once in _Revue Parti Pris_.

“Beyond Anticosti, the wind becomes mild, Cartier smells fresh water, probably grimaces, because on the way to the Orient, to avoid deviating, it is necessary to keep one's rudder in the salty water. He therefore looks for another passage, surveys the coasts, doesn't find one and continues, leaving the imagined archipelago to enter a vast continent through the mouth of a new Orinoco. It seems to me that this is a rather important moment in history, the first glimpse of North America… […] In the absence of China, he is looking for Peru since he has discovered another America.”[^fn5]

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Orinoco.png" title="Orinoco" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Visualization produced with the Latitude/Longitude Distance Calculator by Meridian Outpost.
</div>

In this explicitly colonial imaginary, Orinoco is a placeholder for the gulf of Saint Lawrence, and Peru is a placeholder for China. And elsewhere in that same text, Ferron imagines how sixteenth-century colonials would have first apprehended the New World:

“We did not think of a continental facade but rather the ornament of some large islands ahead of an immense archipelago where the prestigious country described by Marco Polo would have been located.”[^fn6]

Just as Jacques Cartier is made equivalent to Marco Polo, the territory colonially known as New France, then present-day New France could be a placeholder for China. These are the Islands that Repeat Themselves.

I initially included Venice as one more set of islands in this corpus. Venice’s famous merchant is not the only tie to the conference in October. NER helped me surface Venezia, Piazza San Marco, and Palazzo Ducale as unique place names. I did not strictly need computational tools to detect these place names, but I admit without named entity recognition and context extraction, I would not have expected them, and without Wikidata ID and coordinates, I very well might not have recognized or remembered them, even if my eyes had glazed over them.

To conclude, I briefly gloss the passage from a text titled “Beatniks, Vietniks, Québecniks : gauchisme à gogo?” by Jan Depocas, where Venezia, Piazza San Marco, and Palazzo Ducale appear together, as further evidence the geographic expanse represented in my corpus is in fact placeholders cluttering together.

“It is not in New York, nor even somewhere north of New York, northeast of Ottawa and... at the far west of China, nor even in Venice West, USA, but in... Venice, I admit, in Venice where I was returning from Greece and Istanbul. It is in Venice, the disused crossroads of the East and the West, that I could only know, [...] that I, a Québécois, was from there: from the last white colony of the Far Occident [...] by browsing, in Piazza San Marco, under the gothic arcade of the white Palazzo Ducale, where a colorful little book fair was bustling that evening — by browsing, for the first time, from Marx [...] ‘Venice la Rouge’ …”[^fn7]

Here we get a broad geographic expanse that is only coherent in space in specific historical and racialized context. We get East meeting West, that historic binary with China and Venice Beach, California, as the antipodes, but American, Vietnamese and Quebecois youth juxtaposed in the same enumeration in the title. We get a description of a white youth learning of his colonial subjecthood far from home in a turn of phrase that I can only characterize as colonial hallucination : “the last white colony of the Far Occident.”

[^fn1]: Jean-Daniel Lafond, _La Manière n_gre ou Aimé Césaire, chemin faisant_, 1993, 13, my translation.
[^fn2]: Antonio Benítez-Rojo, _The Repeating Island: The Caribbean and the Postmodern Perspective_, 1989, 3.
[^fn3]: Martin Lewis and Kären Wigen, _The Myth of Continents: A Critique of Metageography_, 1997, ix.
[^fn4]: Lafond, 13, my translation.
[^fn5]: Jacques Ferron, "La Brèche", my translation.
[^fn6]: Ferron, my translation. 
[^fn7]: Depocas, my translation.

> Acknowledgements
> Thank you to Merve Tekgürler for making this project prototype possible in Fall 2023. Thank you to Quinn Dombrowski for initially coding the scraper in Winter 2023. Thank you to Em Ho for improving on Dombrowski's scraper and growing the corpus in Summer 2024. My collaboration with Ho was made possible by the Stanford School of Humanities & Sciences through a Graduate Research Opportunity Grant.
