---
layout: post
title:  "Announcing CLTK 2.0: NLP for *all* pre‑modern languages"
date:   2025-09-27 21:00:00
categories: blog
author: Kyle P. Johnson
---

Using the same API and architecture patterns, I have rewritten almost every line of code. A small breakthrough I found is that generative LLM are able to provide the core NLP tasks that this project aims for: part-of-speech and dependency parsing. The potentially enormous breakthrough is even generalist models (e.g., ChatGPT, Llama) can perform this task for a multitude, if not the majority, of ancient, classical, and medieval languages.

# Features

- Generative models support 105 languages! [See here for all of them](https://docs.cltk.org/reference/cltk/languages/pipelines/#cltk.languages.pipelines.MAP_LANGUAGE_CODE_TO_GENERATIVE_PIPELINE)
- For backend, one may choose OpenAI/ChatGPT, Mistral, or any model that Ollama runs (e.g., Llama). I have added support for Ollama's (paid) cloud service (though have not tested this yet).
- The project keeps the Stanza backend [see all 11 languages supported with the Stanza backend here](https://docs.cltk.org/reference/cltk/languages/pipelines/#cltk.languages.pipelines.MAP_LANGUAGE_CODE_TO_STANZA_PIPELINE)
- The public API remains nearly unchanged from previous v1.x.
- Likewise the architecture is nearly the same, too, though certain processes have dropped and others added in this new, v2.. See page 22-25 of our publication: https://aclanthology.org/2021.acl-demo.3.pdf
- We still map the string labels into valid UD POS tags, morphological features, and dependency syntax labels. Arguably the data types are easier to use. It is certainly easier to make corrections when incorrectly named outputs come from a model [see here for examples](https://docs.cltk.org/reference/cltk/morphosyntax/ud_features/#cltk.morphosyntax.ud_features.normalize_ud_feature_pair).
- The architecture is clean, it builds fast, and has extensive logging; all of which mean that debugging and writing tested patches will be much faster than before.

# Installation

```python
$ pip install "cltk[openai,stanza,ollama]"
```

See more at: <https://docs.cltk.org/quickstart/#install>.

# Use

## ChatGPT

Create a file `.env` and put in it your ChatGPT key (`OPENAI_API_KEY`), i.e.:

```
OPENAI_API_KEY=YOURSECRETKEYDONTSHARE
```

Then run:

```python
from cltk import NLP
nlp = NLP("lati1261", backend="openai", suppress_banner=True)
doc = nlp.analyze("Gallia est omnis divisa in partes tres.")
print(doc.words)
```

## Ollama

Install (Ollama)[https://ollama.com/], start the application, and [find a model](https://ollama.com/search) to download. In my testing I have used `llama3.1:8b`.

```python
from cltk import NLP
nlp = NLP("lati1261", backend="ollama", suppress_banner=True)
doc = nlp.analyze("Gallia est omnis divisa in partes tres.")
print(doc.words)
```

Should anyone try Ollama's cloud GPU service, add `OLLAMA_CLOUD_API_KEY` and your key to the `.env`.

## Stanza

```python
from cltk import NLP
nlp = NLP("lati1261", backend="stanza", suppress_banner=True)
doc = nlp.analyze("Gallia est omnis divisa in partes tres.")
print(doc.words)
```

# Weaknesses and gaps

- I should note that this is closer to "beta" than "production" software. I have decided to promote it to `master` anyways due to my limited time and limited number of test users.
- I have not added Spacy integrations yet, but hope to do so. For the past several years, I have had consistent difficulties getting it to install correctly on users' machines.
- I have no benchmarks for the generative LLM, but hope to do so! When inspecting the generative LLM, it seems they have been trained on the entirety of the Universal Dependencies treebanks, and if so then new benchmarks ought be created, even if small.
- Calls to ChatGPT are slow! I am using async calls (per sentence) to speed things up, but this only goes so far. It's relatively expensive, too, to label large amounts of text, but it's reasonable to expect that costs will go down over time.
- The mapping of non-UD (but correct) labels is not complete. I have put a significant number of hours correcting those that come from ChatGPT 5 for Ancient Greek and Latin. I observe that other languages emit a number of these mistakes, but I do not feel confident mapping them. This will take a number of experts if it's to ever be complete.
- There is some valuable code that did not (or has not yet) made its way over, including some wrappers around Collatinus, TLGU, and more. I have not made any firm decisions about these yet.

# Legacy support

I insistent in stating version 1.x will not receive any more support. Yes, I should have given better warning.

As we did in the transition of v0 to v1, I have preserved the code, docs, and packages on PyPI. So if you do not want to upgrade to v2, you will always have available the last 1.x version with `pip install cltk==1.5.0`. Its docs are now at <https://v1.cltk.org> and the code is at branch `v1`. Likewise the initial generation of the CLTK is at branch `v0`, <https://v0.cltk.org/>, and `pip install cltk==0.1.121`.