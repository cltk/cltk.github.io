---
layout: post
title:  "On under-resourced languages and the CLTK"
date:   2018-04-27 09:00:00
categories: blog
author: Kyle P. Johnson
---

A central aim of the CLTK is complete NLP coverage of all pre-Modern languages. In practice, however, this ambitious goal needs to be tempered by availability of language resources, digital and human. With some frequency, we are approached by potential contributors who hope to pitch in by adding full (or significant) NLP tools for a given language. Over the past six years, those of us centrally involved in the project have learned a great deal about what characteristics distinguish a successful from unseccessful project. I'll describe these characteristics in some detail below, but to summarize, a successful "add-a-language" project for the CLTK depends already-available digitized data.

[comment]: <> (define an under-resourced language)
Languages which lack such digitized data that I'll call "under-resourced." To help illustrate, I'll borrow [a definition of a "language resource"](http://www.elra.info/en/about/what-language-resource/).<sup>[1](#myfootnote1)</sup>

> The term *Language Resource* refers to a set of speech or language data and descriptions in machine readable form, used for building, improving or evaluating natural language and speech algorithms or systems, or, as core resources for the software localisation and language services industries, for language studies, electronic publishing, international transactions, subject-area specialists and end users.

 >Examples of Language Resources are written and spoken corpora, computational lexica, terminology databases, speech collection, etc.

 For the CLTK, language resources normally take the form of static data sets, such as text corpora (annotated or plaintext), [treebanks](https://en.wikipedia.org/wiki/Treebank), and lexica (dictionaries of various sort). Such resources also take the form of algortihms, which are usually rules-based and optionally rely upon data sets. For an example of the former sort of algorithm, see [Luke Hollis's stemmer for Latin](https://github.com/cltk/cltk/blob/9deebf3ff050ab6c12c0c5ceb953bc8ecce21ed0/cltk/stem/latin/stem.py
), which is itself a Python-language implementation of rules defined by peer-reviewed scholarship by other scholars (Schinke et al., 1996). For example of rules-plus-data, [Todd Cook's Latin syllabifier](https://github.com/cltk/cltk/blob/9b9cdb42dcc1c707ab3db3ef8214837bb7c262b5/cltk/prosody/latin/Syllabifier.py#L36) first defines a variety of character types in [ScansionConstants.py](https://github.com/cltk/cltk/blob/9b9cdb42dcc1c707ab3db3ef8214837bb7c262b5/cltk/prosody/latin/ScansionConstants.py) and then uses these in `Syllabifier.syllabify()`. Examples of well-resourced languages, in both data and algorithm, include Ancint Greek and Latin, for which not coincidentally the CLTK has excellent coverage.


[comment]: <> (explain why resources are critical)
The CLTK is an NLP project, and it eschews ventures outside of this mission. Casually speaking, this means that the CLTK is neither a data annotation or a user-facing display project. In reference to the classic [three-tier architecture](https://en.wikipedia.org/wiki/Multitier_architecture), the CLTK exclusively restrained to the middle *presentation tier*, relying upon the *data storage tier* as upstream dependencies and adopted downstream by the *presentation tier*. The CLTK has a vested interest in the health of data storage and presentation tiers, however it is simply the case that our project's core NLP task, smartly leveraging already available data, is formidable enough. To satisfy the needs of downstream makers of applications, we whaat we hope are sensible and well documented APIs. To illustrate briefly the challenges relatively simple data creation, I am reminded of several students whose natives tongues were, respectively, Telgugu and Kannada. They approached with proposals to do NLP in these languages, and when I pointed out they'd need data, they came up with the idea of doing OCR to obtain plaintext corpora. After all, plenty of digitized book images could be found online. However, preliminary experiments by each students demonstrated that OCR in non-Latin characters was of very low accuracy. The creation of annotated texts is a laborious in the extreme, requiring equal degrees of passion and technical expertise.

Having explained what a language resource is and why they are so important to the CLTK, I will here explain the states in which under-resourced languages (especially dead) exist and how a potential contributor can discover whether a language of interest is one. Languages generally fail to meet the minimum bar of "resourced" due to one or more of the following attributes:
1. resources are not digitized; 
2. resources are only available under non-free licenese; 
3. resources are available but do not amount to a "critical mass" around which serious NLP tooling may be developed.

[comment]: <> (1. do not exist)
First, simply enough, if data has not been digitized, then it is not available for computational processing. As mentioned above, digitized images of books is quite a distance still from even plaintext files of digitized characters.

[comment]: <> (2. under non-free licenses)
Second, non-free data poses a significant problem. My use of "free" here corresponds generally to that laid down by Richard Stallman as something more particular than no-cost and open source:

> When we call software “free,” we mean that it respects the users' essential freedoms: the freedom to run it, to study and change it, and to redistribute copies with or without changes. This is a matter of freedom, not price, so think of "free speech," not "free beer." (["Why Open Source misses the point of Free Software"](https://www.gnu.org/philosophy/open-source-misses-the-point.html))

Intended to support rigorous quantitative scholarship, users of the CLTK simply must have the ability to manipulate and redistribute the data used to create results. Contrary to the understanding of many humanists I have met, "open access" and "open source" resources that retain have proprietary licenses are unstable foundations for serious projects like ours, the legacy of which (we hope) will be measured in decades, not years. It is undeniably practical for a scholar to quickly publish an article which uses proprietary data or software, however his results will likely not be reproducible, frozen in time, and lost to history.

[comment]: <> (3. some, but not a critical mass; but what is a critical mass?)
Third, The line between a resourced and under-resourced language is not always clear-cut. For exmaple, there could be annotations available, however they be of a rather small in number (the case with [Tibetan POS](https://github.com/cltk/tibetan_pos_tdc)); or some data sets be very robust for some tasks (e.g., a lexicon and word-lookup) however be completely lacking treebanks (Pali); or in the absence of any digitized data, certain within-reach algorithms may be written. Todd's Latin syllabifier, mentioned above, is a successful example of someone creating a new algorithm specifically for the CLTK. In this particular case, I attribute Todd's success a combination of the following factors: his knowing the Latin language well, the language being very well researched in general, other lower-level NLP tools upon which he could leverage if necessary, the task being narrowly scoped, and his own programming expertise.

[comment]: <> (What kind of data-creation should be done by the CLTK? example: stopwords, semi-supervised ML tagging)
Is there some limited data creation that could fall within scope of the CLTK? We have had some success, for example, in generating stopword lists either according to specific algorithms minimizing manual curation (e.g., run tf-idf on a corpus and removing nouns) or with very narrow scope (e.g., typing every possible inflection of a definite article). I don't want to preclude the possibility of others, so I would generalize that this issue ought to be evaluated on a case-by-case basis.

[comment]: <> (the sweet spot, for most exploitation: well resourced languages which are not currently covered well (or at all) by the CLTK; ideas: maybe Hebrew, Sanskrit, Chinese, however each pose significant challenges -- though not insurmountable, we hope -- which potential contribs need to account for)

All the above is intentionally discouraging of those who might want to disembark on an ill-fated proposal to add new language support to the CLTK. On the flip side, we ought to highlight that there are several shining examples of languages I might consdider well-resourced and not currently covered by the CLTK. Those are, in no particular order: Sanskrit, Hebrew, Classical Chinese, and Old English.<sup>[2](#myfootnote2)</sup> To conclude, I encourage those who would like to work with a mentor from our team, to first consider and have preliminary answers to the following questions:
1. What free data sets are available? If there any non-free or ambiguously licensed language resources, are they important enough to researchers that we would leverage them?
2. What NLP algorithms can I write with this data?
3. What free NLP algorithms have already been written? Do I have the skills, approximately, re-implement them?
4. What data am I missing and is it reasonable to create this data within the project? Define very precise scoping and make an effort estimate.
5. Do you have the language skills to validate your own research? If not, have you identified another (say, a professor) who would be able to help? Please remember that the CLTK is exclusively interested in the *pre-modern* form of a language; so for example even though Hindi may be considered an "classical" language, its form as spoken today differs greatly (or so I am told) from how it was written 1,000 years ago.
6. How do you rate your skills in programming in Python, machine learning, and NLP? We have and do work with various types of specialists (some more human language, some more computer), however knowing your particular background helps to pair you with the right mentor.

With these questions answered, even if not perfectly, the core CLTK will be able to respond with concrete advice, criticism, and recommendations of further resources.


<a name="myfootnote1">1</a>: From the Under-resourced Languages group of the European Language Resources Association (ELRA).

<a name="myfootnote2">2</a>: Old and Middle English have good footing, due to the major pre-modern Germanic contributions by Eleftheria Chatziargyriou and Clément Besnier, however relative to the huge amounts of source data, lots of valuable work remains.

