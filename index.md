---
title: Overview . SemEval 2016 Task 10. Detecting Minimal Semantic Units and their Meanings (DiMSUM)
---

# Task Overview

The __DiMSUM__ shared task is concerned with predicting, given an English sentence, a broad-coverage representation of lexical semantics. The representation consists of two closely connected facets: a segmentation into __minimal semantic units__, and a labeling of some of those units with semantic classes known as __supersenses__.

For example, given the POS-tagged sentence

>  I<sub>`PRP`</sub>  googled<sub>`VBD`</sub> restaurants<sub>`NNS`</sub> in<sub>`IN`</sub> the<sub>`DT`</sub> area<sub>`NN`</sub> and<sub>`CC`</sub> Fuji<sub>`NNP`</sub> Sushi<sub>`NNP`</sub> came<sub>`VBD`</sub> up<sub>`RB`</sub> and<sub>`CC`</sub> reviews<sub>`NNS`</sub> were<sub>`VBD`</sub> great<sub>`JJ`</sub> so<sub>`RB`</sub> I<sub>`PRP`</sub> made<sub>`VBD`</sub> a<sub>`DT`</sub> carry<sub>`VB`</sub> out<sub>`RP`</sub> order<sub>`NN`</sub>

the goal is to predict the representation

>  I  googled<sub>`communication`</sub> restaurants<sub>`GROUP`</sub> in the  area<sub>`LOCATION`</sub> and  Fuji`_`Sushi<sub>`GROUP`</sub> came`_`up<sub>`communication`</sub> and  reviews<sub>`COMMUNICATION`</sub> were<sub>`stative`</sub> great so I  made`_` a  carry`_`out<sub>`possession`</sub> `_`order<sub>`communication`</sub>

where `lowercase` labels are verb supersenses, `UPPERCASE` labels are noun supersenses, and  `_` joins tokens within a multiword expression. (_carry_`_`_out_<sub>`possession`</sub> and _made_`_`_order_<sub>`communication`</sub> are separate MWEs.)

The two facets of the representation are discussed in greater detail below. Systems are expected to produce the both facets, though the manner in which they do this (e.g., pipeline vs. joint model) is up to you.

Gold standard training data labeled with the combined representation will be provided in two domains: __online reviews__ and __tweets__. (Rules for using other data resources in [data conditions](#data-conditions).) Blind test data will be in these two domains as well as a third, surprise domain. The domain will not be indicated as part of the input at test time. The three test domains will have equal weight in the overall system scores; see [scoring](scoring.html).

## Minimal semantic units

The word tokens of the sentence are partitioned into basic units of lexical meaning. Equivalently, where multiple tokens function together as an idiomatic whole, they are grouped together into a __multiword expression__ (MWE). MWEs include: nominal compounds like _hot dog_; verbal expressions like _do away with_ 'eliminate', _make decisions_ 'decide', _kick the bucket_ 'die'; PP idioms like _at all_ and _on the spot_ 'without planning'; multiword prepositions/connectives like _in front of_ and _due to_; multiword named entities; and many other kinds.
  - Input word tokens are never subdivided.
  - Grouped tokens do not have to be contiguous; e.g., verb-particle constructions are annotated whether they are contiguous (_make up_ the story) or gappy (_make_ the story _up_). There are, however, formal constraints on gaps to facilitate sequence tagging.
  - Combinations considered to be statistical collocations (yet compositional in meaning) are called "weak MWEs", distinguished from MWEs with idiomatic meanings ("strong MWEs"). Otherwise, different categories of MWEs are not explicitly annotated.

Refer to [this LREC 2014 paper](http://www.cs.cmu.edu/~nschneid/mwecorpus.pdf) for details of the MWE annotation.

## Semantic classes

These are broad-coverage lexical categories known as "supersenses".
  - There are 26 noun supersenses, including `PERSON`, `LOCATION`, `TIME`, `FOOD`, and `COMMUNICATION`. They cover common nouns as well as proper names (named entities).
  - There are 15 verb supersenses, including `motion`, `social`, and `communication`.
  - Supersense annotations always respect strong MWE annotations: the supersense class applies to the entire MWE as a unit. Of MWEs, all and only the ones that holistically function as a noun or verb expression are labeled with a supersense.
  - Single-word noun and verb tokens also receive supersenses. E.g., instances of _hot dog_ and _hamburger_ would both receive the `FOOD` label.

Refer to [this NAACL 2015 paper](http://www.cs.cmu.edu/~nschneid/sst.pdf) for details of the annotation of supersenses on top of MWEs.

## Data conditions

There will be three conditions according to which systems will be compared.

- In the __open__ condition, systems may use any and all available resources.
- To facilitate a controlled comparison of algorithms, in the __(semi-)supervised closed__ conditions, systems may only use specific data resources, namely:
  * the labeled training data
  * a provided set of word clusters (Brown clusters)
  * the [English WordNet](http://wordnet.princeton.edu/) lexicon
  * in the __semi-supervised closed__ condition, the [Yelp Academic Dataset](https://www.yelp.com/academic_dataset)

System submissions will specify which of these datasets were used, and this will determine which competition(s) it is entered into. Each team is allowed to submit up to 3 systems—one in the supervised closed condition, one in the semi-supervised closed condition, and one in the open condition.

A new test set will be annotated for this task and distributed to participants shortly before the start of the evaluation period.

## Baseline