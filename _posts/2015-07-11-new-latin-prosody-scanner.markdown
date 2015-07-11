---
layout: post
categories: blog
title:  "Prosody"
date:   2015-07-11 21:28:00
author: Kyle P. Johnson
---


# Latin Prosody Tool

After six months of development, Bradley Baker and I have completed a Latin prosody module for the CLTK with the helpful guidance of Kyle Johnson. The module accepts any macronized Latin text and returns to the user the resulting scansion. 

We designed this module as a component for my forthcoming undergraduate thesis on the prose rhythms of Demosthenes and Cicero. Since my explicit intention was to scan prose rhythms, the program ignores some features of poetic prosody such as hiatus. Future versions may include extra features for scanning Latin poetry.

The basic algorithm used in the module is rather straightforward. A text is first tokenized and syllabified using a combination of the CLTK’s existing modules and methods created specifically for the task of prosody. Common rules of elision are observed. At this point, the module handles a list of lists, with each list containing the syllables of a corresponding sentence in the text. These list of syllables are then scanned individually according to the standard rules of prosody. If a syllable is either long by position or long by nature, a ‘-‘ is appended to the output string; all other syllables are considered short and so a ‘u’ is appended to the output string for them. The module then returns a final list containing the scansion of each sentence in the text.

Before releasing this module, I conducted an accuracy test to determine it’s reliability. The test used the first 243 syllables of Cicero’s *Second Philippic* as the control. Out of these 243 syllables, the module correctly identified 234. The error rate of 0.037 indicates that the module is 96.30% accurate. Currently, the program does not scan identical syllables in the same sentence separately, which is to blame for all 9 errors in the test. Future versions will correct this issue. Other features for future versions will include a Greek scansion module and a Latin macronizer.



Tyler Kirby,

New College of Florida

joseph.kirby12@ncf.edu



# Docs for Latin prosody module: http://docs.cltk.org/en/latest/latin.html#prosody-scanning

Code repository for Latin prosody module: https://github.com/kylepjohnson/cltk/tree/master/cltk/prosody


Academic resources for scansion and prose rhythm:
Greenough, J. B., A. A. Howard, G. L. Kittredge, and Benj. L. D'ooge, eds. Allen and Greenough's New Latin Grammar. Boston, Mass.: Ginn &, 1904. §602-614.


"Prose Rhythm." In Philippics I-II, edited by J. T. Ramsey, 20-22. Cambridge: Cambridge University Press, 2003.



Hornblower, Simon, and Antony Spawforth. "Prose Rhythm." Oxford Classical Dictionary. Third Edition Revised ed. Oxford University Press, 2003. 1260-1262.

More approachable introduction to scansion:


Turpin, William. "Scansion." Dickinson College Commentaries. Accessed July 11, 2015. http://dcc.dickinson.edu/ovid-amores/scansion.

The test code will take a few days to write since I need to manually add the 243 syllables scanned by hand and I'm currently in the midst of analyzing the data from the module for my thesis, but I'll send it to you as soon as it's complete. 