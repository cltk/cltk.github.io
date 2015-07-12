---
layout: post
categories: blog
title:  "New Latin prosody scanner"
date:   2015-07-11 21:28:00
author: Tyler Kirby
---

After six months of development, Bradley Baker and I have completed a Latin prose scanner for the CLTK. The module accepts any macronized Latin text and returns to the user the resulting scansion.

We designed it as a component for my forthcoming undergraduate thesis on the prose rhythms of Demosthenes and Cicero. Since my intention was to scan prose rhythms, the program ignores some features of poetic prosody, such as hiatus. Future versions may include extra features for scanning Latin poetry.

The basic algorithm used in the module is rather straightforward. An input string's words are first tokenized (via the CLTK) and syllabified (via a method in the new module). Then, common rules of elision are observed. At this point, the module handles a list of lists, with each containing the syllables of the input sentences. These lists of syllables are then scanned individually according to the standard rules of prosody. If a syllable is either long by position or long by nature, a '¯' is appended to the output string; all other syllables are considered short with a '˘'. The module then returns a final list containing the scansion of each sentence in the text.

Before releasing this module, I conducted an accuracy test (code forthcoming) to determine it’s reliability, using the first 243 syllables of Cicero’s *Second Philippic* as the control. Of these 243 syllables, the module correctly identified 234 (for an accuracy of 96.30%). Currently, the program does not scan identical syllables in the same sentence separately, which is to blame for all 9 errors in the test. Future versions will correct this issue. Other features for future versions will include a Greek scansion module and a Latin macronizer.



Tyler Kirby

New College of Florida

joseph.kirby12@ncf.edu



# CLTK Docs

Docs for Latin prosody module: <http://docs.cltk.org/en/latest/latin.html#prosody-scanning>.

Code for Latin prosody module: <https://github.com/kylepjohnson/cltk/blob/master/cltk/prosody/latin/scanner.py>.


# Approachable introduction to scansion:

Turpin, William. "Scansion." Dickinson College Commentaries. Accessed July 11, 2015. <http://dcc.dickinson.edu/ovid-amores/scansion>.


# Academic resources for scansion and prose rhythm:

Greenough, J. B., A. A. Howard, G. L. Kittredge, and Benj. L. D'ooge, eds. Allen and Greenough's New Latin Grammar. Boston, Mass.: Ginn &, 1904. §602-614.

"Prose Rhythm." In Philippics I-II, edited by J. T. Ramsey, 20-22. Cambridge: Cambridge University Press, 2003.

Hornblower, Simon, and Antony Spawforth. "Prose Rhythm." Oxford Classical Dictionary. Third Edition Revised ed. Oxford University Press, 2003. 1260-1262.