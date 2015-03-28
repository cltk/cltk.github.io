---
layout: post
categories: blog
title:  "Welcome to the CLTK homepage!"
date:   2015-03-28 15:28:00
author: Kyle P. Johnson
---
I am excited to have this little website to bring useful information to users of the CLTK. As the project continues to grow, I hope users can share tutorials, code snippets, etc..

If you are interested in authoring a post, you can send your text (preferably in Markdown) to me by email ([kyle@kyle-p-johnson.com](kyle@kyle-p-johnson.com)) or, better yet, fork [this site's repository](https://github.com/cltk/cltk.github.io), add your post, and make a pull request. This can all be done in-browser on GitHub. While not necessary, to clone this site and run it locally, see [directions for using Jekyll on GitHub pages](https://help.github.com/articles/using-jekyll-with-pages/).

To author a new post, simply add a file to to the `_posts` directory, following the convention `YYYY-MM-DD-name-of-post.markdown` and edit in the header the fields `title`, `date`, and `author` (leave `layout` and `categories` alone). 

One note for future contribs, code snippets such as the following are done with the following syntax.

{% highlight python %}

In [1]: from nltk.tokenize.punkt import PunktLanguageVars

In [2]: from cltk.stop.greek.stops import STOPS_LIST

In [3]: text = """Ἡροδότου Θουρίου ἱστορίης ἀπόδεξις ἥδε, ὡς μήτε τὰ 
   ...: γενόμενα ἐξ ἀνθρώπων τῷ χρόνῳ ἐξίτηλα γένηται, μήτε 
   ...: ἔργα μεγάλα τε καὶ θωμαστά, τὰ μὲν Ἕλλησι, τὰ δὲ βαρβάροισι 
   ...: ἀποδεχθέντα, ἀκλέα γένηται, τά τε ἄλλα καὶ δι' ἣν 
   ...: αἰτίην ἐπολέμησαν ἀλλήλοισι."""

In [4]: p = PunktLanguageVars()

In [5]: tokens = p.word_tokenize(text.lower())

In [6]: [w for w in tokens if not w in STOPS_LIST]
Out[6]: 
['ἡροδότου',
 'θουρίου',
 'ἱστορίης',
 'ἀπόδεξις',
 'ἥδε',
 ',',
 'μήτε',
 'γενόμενα',
 'ἐξ',
 'ἀνθρώπων',
 'χρόνῳ',
 'ἐξίτηλα',
 'γένηται',
 ',',
 'μήτε',
 'ἔργα',
 'μεγάλα',
 'θωμαστά',
 ',',
 'ἕλλησι',
 ',',
 'βαρβάροισι',
 'ἀποδεχθέντα',
 ',',
 'ἀκλέα',
 'γένηται',
 ',',
 'ἄλλα',
 'δι',
 "'",
 'ἣν',
 'αἰτίην',
 'ἐπολέμησαν',
 'ἀλλήλοισι.']

{% endhighlight %}