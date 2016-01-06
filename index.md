---
title: "SemEval 2016 Task 10: Detecting Minimal Semantic Units and their Meanings (DiMSUM)"
---

The __DiMSUM__ shared task at [SemEval 2016](http://alt.qcri.org/semeval2016/) is concerned with predicting, given an English sentence, a broad-coverage representation of lexical semantics. The representation consists of two closely connected facets: a segmentation into __minimal semantic units__, and a labeling of some of those units with semantic classes known as __supersenses__.

For example, given the POS-tagged sentence

>  I<sub>`PRON`</sub>  googled<sub>`VERB`</sub> restaurants<sub>`NOUN`</sub> in<sub>`ADP`</sub> the<sub>`DET`</sub> area<sub>`NOUN`</sub> and<sub>`CONJ`</sub> Fuji<sub>`PROPN`</sub> Sushi<sub>`PROPN`</sub> came<sub>`VERB`</sub> up<sub>`ADV`</sub> and<sub>`CONJ`</sub> reviews<sub>`NOUN`</sub> were<sub>`VERB`</sub> great<sub>`ADJ`</sub> so<sub>`ADV`</sub> I<sub>`PRON`</sub> made<sub>`VERB`</sub> a<sub>`DET`</sub> carry<sub>`VERB`</sub> out<sub>`ADP`</sub> order<sub>`NOUN`</sub>

the goal is to predict the representation

>  I  googled<sub>`v.communication`</sub> restaurants<sub>`GROUP`</sub> in the  area<sub>`n.location`</sub> and  Fuji`_`Sushi<sub>`n.group`</sub> came`_`up<sub>`v.communication`</sub> and reviews<sub>`n.communication`</sub> were<sub>`v.stative`</sub> great so I  made`_` a  carry`_`out<sub>`v.possession`</sub> `_`order<sub>`v.communication`</sub>

Noun supersenses start with `n.`, verb supersenses with `v.`, and  `_` joins tokens within a multiword expression. (_carry_`_`_out_<sub>`v.possession`</sub> and _made_`_`_order_<sub>`v.communication`</sub> are separate MWEs.)

The two facets of the representation are discussed in greater detail below. Systems are expected to produce the both facets, though the manner in which they do this (e.g., pipeline vs. joint model) is up to you.

Gold standard training data labeled with the combined representation is provided in two domains: __online reviews__ and __tweets__. (Rules for using other data resources in [data conditions](#data-conditions).) Blind test data will be in these two domains as well as a third, surprise domain. The domain of each sentence will not be indicated as part of the input at test time. The three test domains will have equal weight in the overall system scores (see [scoring procedure](#scoring)).

## Minimal semantic units

The word tokens of the sentence are partitioned into basic units of lexical meaning. Equivalently, where multiple tokens function together as an idiomatic whole, they are grouped together into a __multiword expression__ (MWE). MWEs include: nominal compounds like _hot dog_; verbal expressions like _do away with_ 'eliminate', _make decisions_ 'decide', _kick the bucket_ 'die'; PP idioms like _at all_ and _on the spot_ 'without planning'; multiword prepositions/connectives like _in front of_ and _due to_; multiword named entities; and many other kinds.
  - Input word tokens are never subdivided.
  - Grouped tokens do not have to be contiguous; e.g., verb-particle constructions are annotated whether they are contiguous (_make up_ the story) or gappy (_make_ the story _up_). There are, however, formal constraints on gaps to facilitate sequence tagging.
  - Combinations considered to be statistical collocations (yet compositional in meaning) are called "weak MWEs", distinguished from MWEs with idiomatic meanings ("strong MWEs"). Otherwise, different categories of MWEs are not explicitly annotated.

Refer to [this LREC 2014 paper](http://www.cs.cmu.edu/~nschneid/mwecorpus.pdf) for details of the MWE annotation.

## Semantic classes

These are broad-coverage lexical categories known as "supersenses".
  - There are 26 noun supersenses, including `n.person`, `n.location`, `n.time`, `n.food`, and `n.communication`. They cover common nouns as well as proper names (named entities).
  - There are 15 verb supersenses, including `v.motion`, `v.social`, and `v.communication`.
  - Supersense annotations always respect strong MWE annotations: the supersense class applies to the entire MWE as a unit. Of MWEs, all and only the ones that holistically function as a noun or verb expression are labeled with a supersense.
  - Single-word noun and verb tokens also receive supersenses. E.g., instances of _hot dog_ and _hamburger_ would both receive the `n.food` label.

Refer to [this NAACL 2015 paper](http://www.cs.cmu.edu/~nschneid/sst.pdf) for details of the annotation of supersenses on top of MWEs.

## Data conditions

There will be three conditions according to which systems will be compared.

To facilitate a controlled comparison of algorithms, in the __(semi-)supervised closed__ conditions, systems may only use specific data resources. 

- In the __supervised closed__ condition, the following are permitted:
  * the labeled training data
  * the [English WordNet](http://wordnet.princeton.edu/) lexicon
  * the following sets of word clusters (Brown clusters):
    - yelpac-c1000-m25.gz from the [English Multiword Expression Lexicons](http://www.cs.cmu.edu/~ark/LexSem/)—this clustering was [induced](http://www.cs.cmu.edu/~ark/LexSem/mwelex/README.md) from the Yelp Academic Dataset; and/or
    - any of the [ARK Tweet NLP clusters](http://www.cs.cmu.edu/~ark/TweetNLP/#resources)
- In the __semi-supervised closed__ condition: all of the above are permitted, plus the [Yelp Academic Dataset](https://www.yelp.com/academic_dataset).
- In the __open__ condition, systems may use any and all available resources.

System submissions will specify which of these datasets were used, and this will determine which competition(s) it is entered into. Each team is allowed to submit up to 3 systems—one in the supervised closed condition, one in the semi-supervised closed condition, and one in the open condition.

A new test set has been annotated for this task. A blind version (input only) has been released in advance of the evaluation period.

## Scoring

Our evaluation script, [dimsumeval.py](https://github.com/dimsum16/dimsum-data/blob/master/scripts/dimsumeval.py), is bundled with the latest data release. Primarily, it reports F-scores for MWE identification, supersense labeling, and their combination. See the documentation in the script for an overview. Further details of the scoring procedure will be announced at a future time.

## Downloads

- __[Training/test data + scripts v1.5](https://github.com/dimsum16/dimsum-data/releases/tag/1.5)__
  * [README](https://github.com/dimsum16/dimsum-data/blob/1.5/README.md)
  * [TAGSET](https://github.com/dimsum16/dimsum-data/blob/1.5/TAGSET.md)
- __Trial data__: Download STREUSLE 2.0 [here](http://www.ark.cs.cmu.edu/LexSem/). This consists of annotated online reviews (it will eventually form part of the training set for the task).
  * Refer to the files streusle.tags and streusle.tags.sst (which contain equivalent information, but in different formats). The formats are described in README.md.
- A __baseline system__ will be provided as well.

## Organization

Please subscribe to https://groups.google.com/group/dimsum16 for announcements about the task.

See the [schedule](http://alt.qcri.org/semeval2016/task10/index.php?id=important-dates) for planned data releases and deadlines.

The organizers are:

* [Nathan Schneider](http://nathan.cl), University of Edinburgh
* [Dirk Hovy](http://dirkhovy.com/), University of Copenhagen
* [Anders Johannsen](http://www.johannsen.com/), University of Copenhagen
* [Marine Carpuat](http://marinecarpuat.weebly.com/), University of Maryland
