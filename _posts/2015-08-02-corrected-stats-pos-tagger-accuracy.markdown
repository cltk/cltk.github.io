---
layout: post
categories: blog
title:  "Updated accuracies for POS taggers"
date:   2015-08-02 15:00:00
author: Kyle P. Johnson
---


Last Fall, I [published on my personal blog](http://kyle-p-johnson.com/blog/2014/12/31/cltk-pos-tagging-cross-validation.html) a series of accuracy scores for the CLTK's POS tagging models. It was recently pointed out to me, by a member of Persus's treebank tagging team, that the training set I used to make the taggers contained duplicates. This resulted in a skewing of the accuracy scores, though not the taggers themselves.

The Perseus files, [kept here for Greek](https://github.com/cltk/greek_treebank_perseus/tree/master/greek_treebank_perseus) and [here for Latin](https://github.com/cltk/latin_treebank_perseus/tree/master/latin_treebank_perseus), contain a single XML for each tagged work and one file, `agdt-1.7.xml` for Greek and `ldt-1.5.xml` for Latin, for all individual files brought together. In training and testing the taggers, I had combined all XML files in the directory, the works plus their compilation, while I should I have included only the complilation files.

I am happy to report that the CLTK taggers have been retrained and that their functionality is near-identical to what they were. This is because the duplicate fields did not give the model any new information about a particular word's POS tag nor those of its neighbor.

The testing corpus I had used, however, was considerably skewed. While cross-validating the scripts, the testing set contained sentences already seen in training. Essentially, the model was being tested on matter it had already seen. With cross-validation run again ([Greek](https://github.com/kylepjohnson/ipython/blob/master/pos_tagging/Cross-validation%20of%20the%20CLTK's%20POS%20taggers%2C%20Greek.ipynb) and [Latin](https://github.com/kylepjohnson/ipython/blob/master/pos_tagging/Cross-validation%20of%20the%20CLTK's%20POS%20taggers%2C%20Latin.ipynb) notebooks), the CLTK's POS taggers' averages (mean over 10 iterations) are as follow.

### Greek accuracy

| Tagger  |  Accuracy |
|---|---|
|  unigram | 0.815227  |
|  bigram |  0.255973 |
| trigram  | 0.195761  |
| 1, 2, 3-gram backoff  | 0.818673  |
|  tnt | 0.828682  |

### Latin accuracy

| Tagger  |  Accuracy |
|---|---|
|  unigram |  0.679296 |
|  bigram |  0.102156 |
| trigram  |  0.075027 |
| 1, 2, 3-gram backoff  | 0.686440  |
|  tnt |  0.700846 |

The TnT algorithm returns the best restuls at 83% accuracy for Greek and 70% for Latin. These are not bad scores, it seems to me, however they are something of a letdown from scores in the high 90's which I claimed earlier!

To get the latest taggers, update your `greek_models_cltk` and `latin_models_cltk` files ([directions here](http://docs.cltk.org/en/latest/importing_corpora.html#importing-a-corpus)).

I give sincere thanks to the Perseus researcher and also apologize to anyone who was mislead by my mistake.