---
layout: post
categories: blog
title:  "New Greek and Latin lemmatizer"
date:   2015-05-10 21:28:00
author: Kyle P. Johnson
---

I'm happy to announce that the CLTK has a new and much improved (faster and more accurate) lemmatizer for the Latin and, now too, Greek languages.

With any lemmatizer, choices must be made about which headword to select in the case that a lemma is ambiguous, that is could belong to more than one headword. In the event of such ambiguity, the previous lemmatizer took whatever headword it found first. Now, the lemmatizer (or rathter the code which created the lemmatizer's key-value data store) takes into consideration all possibilities and selects the headword which has the most frequent occurences in the rest of the list. The code is found [here for Greek](https://github.com/cltk/greek_lexica_perseus/blob/master/transform_lemmata.py) and [here for Latin](https://github.com/cltk/latin_pos_lemmata_cltk/blob/master/transform_lemmata.py). (The original data, I should mention, comes from the Perseus Project.)

The logic I use in the above algorithms is sound, however of course this will not be without flaws. I believe that the best way to improve these lemma-headword lists is to manually write corrections, which will be run over the automatically built file. Any corrections should be submitted to the [CLTK's issues tracker](https://github.com/kylepjohnson/cltk/issues).

Docs for use of the Greek lemmatizer: <http://docs.cltk.org/en/latest/greek.html#lemmatization>

Docs for use of the Latin lemmatizer: <http://docs.cltk.org/en/latest/latin.html#lemmatization>

Code repository for making Greek lemma-headword list: <https://github.com/cltk/greek_lexica_perseus>

Code repository for making Latin lemma-headword list: <https://github.com/cltk/latin_pos_lemmata_cltk>