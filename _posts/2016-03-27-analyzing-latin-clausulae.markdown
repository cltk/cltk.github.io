---
layout: post
categories: blog
title:  "Analyzing Latin clausuale"
date:   2016-03-27 21:28:00
author: Tyler Kirby
---

Latin prose authors since Cicero often employed rhythms at the end of their periods to produce sonorous effects like those found in poetry. These rhythms not only add to the elegance of an author's style, but they even often help characterize a specific style. The prose rhythm preferences of some Roman authors are so distinct that they are often used in matters of textual emendation and author attribution, and so philologists for the last century have spent considerable time cataloguing the rhythm of prose with the hope that better textual judgements may be made with more data.

The CLTK now includes a clausulae analysis module that -- in conjunction with the existing prosody module -- can produce a rhythmic profile of a text.

```python
from cltk.prosody.latin.scanner import Scansion
from cltk.prosody.latin.clausulae_analysis import Clausulae

# A sample analysis of Cicero's First Catilinarian
# NB: The text must be macronized before scanning
with open("~/CiceroCat1.txt") as file_open: 
    text = file_open.read()

scansion = Scansion().scan_text(text)
clausulae = Clausulae().clausulae_analysis(scansion)
print(clausulae)
```

This profile comes in the form of a dictionary in which each key is a clausulae type and each value is the frequency of that clausulae in the text.

```python
{
    'molossus + double trochee': 12, 
    'cretic + double trochee': 6, 
    'choriamb + double trochee': 1, 
    'cretic + double spondee': 1, 
    'heroic': 9, 
    '1st paeon + trochee': 12, 
    'double cretic': 9, 
    'molossus + cretic': 6, 
    'double spondee': 8, 
    'molossus + iamb': 11, 
    '1st paeon + anapest': 0, 
    'dactyl + double trochee': 2, 
    'cretic + trochee': 27, 
    'cretic + iamb': 13, 
    'substituted cretic + trochee': 0, 
    'double trochee': 27, 
    '4th paeon + trochee': 2, 
    '4th paeon + cretic': 3
}
```

The specific clausulae that the module searches for is derived from a list provided in the introduction of John Ramsey's Cambridge commentary on Cicero's *Philippics* I-II. See the table below for the relation between the name of a clausulae, the 'type' which Ramsey assigns to it (note that the heroic clausulae, type 6, is my own addition to the list), and its metrical construction. Short syllables are denoted with 'u', long with '-', and substitution with parentheses.

|Type|Name|Definition|
|---|---|---|
Type 1 | Cretic + Trochee | - u - / - x
Type 1a | Fourth Paeon + Trochee | (uu) u - / - x
Type 1b | First Paeon + Trochee | - u (uu) / - x
Type 1c | Substituted Cretic + Trochee | (uu) u (uu) / - x
Type 1d | First Paeon + Anapest | - u (uu) / u u x
Type 2 | Double Cretic | - u - / - u x
Type 2a | Fourth Paeon + Cretic | (uu) u - / - u x
Type 2b | Molossus + Cretic | - (-) - / - u x
Type 3 | Double Trochee | - u / - x
Type 3a | Molossus + Double Trochee | - - - / - u / - x
Type 3b| Cretic + Double Trochee | - u - / - u / - x
Type 3c | Dactyl + Double Trochee | - u u / -u / - x
Type 3d | Choriamb + Double Trochee | - u u - / - u / - x
Type 4 | Cretic + Iamb | - u -/ u x
Type 4a | Molossus + Iamb | - - - / u x
Type 5 | Double Spondee | - - / - x
Type 5a | Cretic + Double Spondee | - u - / - - / - x
Type 6 | Dactyl + Spondee | - u u / - x

\\
For more information, see [Clausulae analyses docs](http://docs.cltk.org/en/latest/latin.html#clausulae-analysis).