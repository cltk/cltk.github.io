---
layout: post
categories: blog
title:  "Tokenizing Latin text"
date:   2015-08-02 21:32:00
author: Patrick J. Burns
---

*Note: The following is re-posted from Patrick's blog, [Disjecta Membra](https://disiectamembra.wordpress.com/2015/08/01/tokenizing-latin-text/).*

One of the first tasks necessary in any text analysis projects is [tokenization](http://nlp.stanford.edu/IR-book/html/htmledition/tokenization-1.html)—we take our text as a whole and convert it to a list of smaller units, or tokens. When dealing with Latin—or at least digitized version of modern editions, like those found in the [Perseus Digital Library](http://www.perseus.tufts.edu/hopper/collection?collection=Perseus:collection:Greco-Roman), the [Latin Library](http://www.thelatinlibrary.com/), etc.—paragraph- and sentence-level tokenization present little problem. Paragraphs are usually well marked and can be split by newlines (`</n>`). Sentences in modern Latin editions use the same punctuation set as English i.e., ‘.', ‘?', and ‘!'), so most sentence-level tokenization can be done more or less successfully with the built-in tools found in the [Natural Language Toolkit](http://www.nltk.org/) (NLTK), e.g. [nltk.word_tokenize](http://www.nltk.org/api/nltk.tokenize.html). But just as in English, Latin word tokenization offers small, specific issues that are not addressed by NLTK. The classic case in English is the negative contraction—how do we want to handle, for example, “didn't”: [“didn't”] or [“did”, “n't”] or [“did”, “not”]?

There are four important cases in which Latin word tokenization demands special attention: the enclictics “-que”, “-ue/-ve”, and “-ne” and the postpositive use of “-cum” with the personal pronouns (e.g. nobiscum for *cum nobis). The [Classical Language Toolkit](https://github.com/kylepjohnson/cltk) now takes these cases into consideration when doing Latin word tokenization. Below is a brief how-to on using the CLTK to tokenize your Latin texts by word. [The tutorial assumes the following requirements: [Python3](https://www.python.org/download/releases/3.0/), NLTK3, CLTK.]

## Tokenizing Latin Text with CLTK
We could simply use Python to `split` our texts into a list of tokens. (And sometimes this will be enough!) So …

> `>>> text = "Arma virumque cano, Troiae qui primus ab oris"`
>
> `>>> text.split()`
>
> `['Arma', 'virumque', 'cano,', 'Troiae', 'qui', 'primus', 'ab', 'oris']`

A good start, but we've lost information, namely the comma between *cano* and *Troiae*. This might be ok, but let's use NLTK's tokenizer to hold on to the punctuation.

> `>>> import nltk`
>
> `>>> nltk.word_tokenize(text)`
>
> `['Arma', 'virumque', 'cano', ',', 'Troiae', 'qui', 'primus', 'ab', 'oris']`

Using *word_tokenize*, we retain the punctuation. But otherwise we have more or less the same division of words.

But for someone working with Latin that second token is an issue. Do we really want *virumque*? Or are we looking for *virum* and the enclitic *–que*? In many cases, it will be there latter. Let's use CLTK to handle this.

> `>>> from cltk.tokenize.word import WordTokenizer`
>
> `>>> word_tokenizer = WordTokenizer('latin')`
> `>>> word_tokenizer.tokenize(text)`
>
> `['Arma', 'virum', '-que', 'cano', ',', 'Troiae', 'qui', 'primus', 'ab', 'oris']`

Using the CLTK WordTokenizer for Latin we retain the punctuation and split the special case more usefully.

More on tokenization:

* [Processing Raw Text in the NLTK book](http://www.nltk.org/book/ch03.html)
* [CLTK documentation for WordTokenizer](http://classical-language-toolkit.readthedocs.org/en/latest/latin.html#word-tokenization)
* [Text Mining Online](http://textminingonline.com/dive-into-nltk-part-ii-sentence-tokenize-and-word-tokenize)
* [Text Analysis with Topic Models for the Humanities and Social Sciences](https://de.dariah.eu/tatom/preprocessing.html#tokenizing)
