---
layout: post
categories: blog
title: "Descartes meet Python. Python meet Descartes."
date: 2016-06-12
author: Peter Aronoff
---

*Note: The following is re-posted from Peter's website, [ithaca](http://ithaca.arpinum.org/2016/06/08/latin-lexicon.html).*

This summer I'm working on [a commentary on Descartes' *Meditationes de prima philosophia*][mdpp], usually known in English as his *Meditations on First Philosophy*. (Though actually a better translation is the less literal *Metaphysical Meditations*, which is how it's usually translated into French.) In addition to providing a text and commentary, I plan to produce a vocabulary.  This is a time-consuming and error-prone job, so naturally I want to offload at least some of the grunt work to a computer. (See [laziness as virtue for programmers][laziness].)

[mdpp]: https://bitbucket.org/telemachus/descartes-meditations
[laziness]: http://c2.com/cgi/wiki?LazinessImpatienceHubris

As a start, I took at look at [The Classical Language Toolkit][cltk]. CLTK provides a set of natural language processing utilities for ancient texts. Although the Latin tools in CLTK are aimed primarily at classical material, the neo-Latin that Descartes wrote is largely classical in style and vocabulary. The following short Python script [tokenizes][tokenize] and [lemmatizes][lemmatize] Descartes' first meditation. This gets us well on our way since the script reduces its input text to a list of unique words reduced to their dictionary entry. That is, suppose that Descartes wrote *scīre* and *sciēns* (an infinitive meaning *to know* and a participle meaning *knowing*), the script would output only *sciō*, the shared dictionary entry of those two forms.

[cltk]: http://cltk.org/
[tokenize]: https://www.ibm.com/developerworks/community/blogs/nlp/entry/tokenization?lang=en
[lemmatize]: https://en.wikipedia.org/wiki/Lemmatisation

```python
from cltk.stem.lemma import LemmaReplacer
from cltk.stem.latin.j_v import JVReplacer
from cltk.tokenize.word import WordTokenizer

meditatio_prima = open("./meditatio-prima.txt", "r").read()

jv = JVReplacer()
meditatio_prima = jv.replace(meditatio_prima)

t = WordTokenizer("latin")
l = LemmaReplacer("latin")

words = l.lemmatize(t.tokenize(meditatio_prima))
words = sorted(list(set(words)))
print("\n".join(words))
```

It would be even better if we could take the dictionary entries from this first script and feed them to a program that would give us definitions. So that's what I worked on next. The [Perseus Digital Library][perseus] has opensourced [the XML version of *A Latin Dictionary* edited by Lewis and Short][ls]. I'd prefer the modern *Oxford Latin Dictionary*, but Lewis and Short is still an outstanding Latin dictionary. Once again, a relatively short Python script will parse the XML and match the words from Descartes' text with their Lewis and Short dictionary entries.

[perseus]: http://www.perseus.tufts.edu/hopper/
[ls]: https://github.com/PerseusDL/lexica/tree/master/CTS_XML_TEI/perseus/pdllex/lat/ls

    from sys import stderr
    from lxml import etree as ET

    # Helper functions
    def warn(*args, **kwargs):
        '''Print message to stderr'''
        print(*args, file=stderr, **kwargs)

    def inner_text(node):
       '''Return all inner text of an XML node'''
       return ''.join([text for text in node.itertext()])

    xml = ET.parse('no-entities-ls.xml')
    entries = xml.xpath('//entryFree')
    lewis_short = {}
    for item in entries:
        lewis_short[item.attrib['key']] = inner_text(item)

    # Load the vocabulary words that we're searching for
    words_file = open('meditatio-words.txt', 'r')
    wanted_words = words_file.read().splitlines()
    words_file.close()

    # Work through the words we want, trying to match their meanings
    for wanted in wanted_words:
        if wanted in lewis_short:
            print('%s' % (lewis_short[wanted]))
        else:
            warn('%s has no entry.' % (wanted))

This is worth walking through. After defining two small helper functions, the next block of code parses the Lewis and Short XML file and stores the entries in a Python dictionary. (Since *dictionary* is now very ambiguous, I'm going to call the data structure a hash from here on in.) The keys of the hash are lookup words, and the values are full entries from Lewis and Short. Since lookup words are just what our first script gave us, we load those into a list and iterate over them. For each word in Descartes, we test whether it's in the hash. If it is, we print out the entry from Lewis and Short. If it's not in the hash, we tell the user that no matching entry was found. (The "no item found" messages are printed to `stderr` instead of `stdout`, so that if the user wants they can print the two output streams to different places. E.g., `python3 vocabulary-builder.py 1>descartes-vocabulary.txt 2>missing-words.txt`)

This is a terrific amount of progress for two short Python scripts. In addition, I extracted the textual data from the XML file and saved that in plain text format. This allows for lots of other potential uses of Lewis and Short without having to deal with the XML. (The plain text is available [on GitHub, under a CC license that allows for further changes][ls-plaintext].)

[ls-plaintext]: https://github.com/telemachus/plaintext-lewis-short

## What's next?

There's a lot of room for improvement. Here's a quick list of things I'm working on or would like to see.

+ Ambiguous words in Lewis and Short have numbers in their XML keys to distinguish them. E.g., there's a verb *adeō* and an adverb *adeō*, and their XML keys are *adeo1* and *adeo2* respectively. The CLTK lemmatizer also numbers ambiguous words, but I'm not sure whether the two systems are the same. For what I'm doing here, it would be ideal if they were, but there may be other considerations at play. Still, it would be worth investigating how much the two numerations overlap and whether CLTK could mimic Lewis and Short.
+ The vocabulary builder spends most of its time parsing the XML file, which is very large and rather complex. As an alternative, it would be simpler to store Lewis and Short in a simple CSV file. (This would make reading in the data trivial.)
+ On the other hand, the way that I extracted the textual data from the XML loses some information. In particular, the details of the section breakdowns in Lewis and Short are not stored as text, but only in the structure and attributes of the XML tags. My method of brute-force extraction didn't preserve those structural signposts. For my purposes, that's fine, but other people may prefer to try to get more structure out of the XML.
+ Lewis and Short have very odd conventions for macrons. First, they very frequently use the brevis to show where vowels are short. E.g., *ăgō*. That's nearly never useful for students. Second, and much worse, they don't provide macrons in several cases where macrons can be assumed as the norm. E.g., the first principal part of verbs or third declension nouns ending in -*tiō*. These cases, and many others, never have macrons. So in fact, the entry for that verb actually starts with the form *ăgo*: an unhelpful brevis and a missing macron. Both would likely confuse contemporary students. That's enormously frustrating, though luckily there is [an excellent Latin macronizer available on GitHub that might help with this problem][macronizer], and it's easy enough to remove all the brevis marks via simple scripts.
+ Finally, as helpful as all of this may be, it's not what I really need. To build a student glossary, I need to strike a balance: (i) I should provide general and basic meanings for the words I gloss, but (ii) I have to make sure to include any specific meanings necessary for the text at hand. (Student glossaries that only provide the meanings for the text at hand are the worst. They should be illegal.) But what these programs provide is far, far more than that. The result now is the entire dictionary entry for each word. Lewis and Short is a detailed, scholarly dictionary. The entries for core words in the language (e.g., *ferō* or *et*) can go on for column after column of small, single-spaced writing. So what I'd need to do is trim these entries down to something more manageable. That's fine, but it would still require a fair amount of work—though nowhere near as much as doing it all from scratch.
+ These command-line scripts are great for me, but for most people they would be a pain. Eventually, it would probably be good to wrap this in a web application or a desktop application. A user could upload the text they wanted to gloss and the application would give them a vocabulary list. (This would be something like [Haverford's terrific Bridge][bridge], but for any Latin text. It's obviously a tall order.)

[macronizer]: https://github.com/Alatius/latin-macronizer
[bridge]: http://bridge.haverford.edu/

As usual, trying to make a computer do your work for you is fun, helpful, and exhausting. I got to learn some Python, and the tokenizer and lemmatizer results alone will save me hours compared to doing it by hand. At the same time, I've only scratched the surface and there's far more work to do. But I'm excited to continue learning Python and to improve the glossary-maker scripts that I've started here.
