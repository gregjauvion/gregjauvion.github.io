---
layout: post
title:  "Neural Networks in Natural Language Processing"
date:   2017-05-23 10:58:21 +0200
categories: jekyll update
---

This post provides a brief overview on the use of neural networks in natural language processing.

First, let's define the important terms :
* A neural network is a mathematical model inspired by the way the brain processes information. In this post, I will describe the structure of neural networks used in natural language processing, but I will not detail how to train them efficiently, as the training methodologies are described in a lot of resources.
* Natural language processing is a field focused on developing methods to program computers for making them able to process and understand human language.

# What kinds of practical problems can natural language processing help solve ?

This section describes some practical problems where natural language processing is used :
* Document summarization : summarizing automatically a document, by detecting only the most needed parts of it
* Document clustering : this task consists of grouping a large number of documents into clusters. This can be used for example in a search engine : the results of a query can be grouped to make easier their browsing
* Automatic translation

# How to build a mathematical representation of the language ?

All the models we will consider hereafter use a word space, in which each word will be a vector. This section describes some popular methods to define this word space.

## Using one-hot vectors to represent words

The simplest word space is a space where each dimension corresponds to one word. Each word is therefore represented by a vector where the component corresponding to this word is set to 1, the others being set to 0 (a one-hot vector).
This method is particularly simple but leads to very high-dimensional spaces, the number of different words in English vocabulary being for example more than 100.000. In practice, very unfrequent words or words which do not appear in the dataset considered are often discarded, which can reduce the number of dimensions needed for an accurate representation.
An other weakness of this method is that the representation of a word does not take into account the sentence in which it appears.

Using word embeddings instead of one-hot vectors will solve both issues identified here.

## Using word embeddings

Here, the words are represented by dense vectors (i.e most of their components are non 0), but in a much lower-dimensional space. In practical applications, the dimension of word embeddings may vary from 50 to several hundreds.

These methods have two noticeable advantages :
* The dimensionality of the problem can be reduced, compared to the use of one-hot vectors
* They take into account the context in which the words appear, which is a wanted behaviour in most applications

A lot of methods have been developed to produce word embeddings (see wikipedia for this). One which is widely used, Word2Vec (created by a team at Google), is based on a neural network, and we will describe it briefly here.

### Word2Vec

The idea is to build a feedforward neural network to predict a word depending on its context, its context being the words surrounding.
Comment est défini le contexte ? Mieux : fenêtre symétrique autour du mot à prédire. Soit CBOW (moyenne des mots autour), soit vector concatenation (concaténation des mots autour), soit mise du décalage avec le mot (the:+2 et pas the seulement), retirer les mots fréquents...
Ou alors on parse la phrase avec un dependency parser, et le contexte est supposé être les mots autour dans le parse tree.
window size : important

### Glove

There is also Glove (created by the Stanford NLP group).

# How a document is represented ?

Like words, documents will be represented as vectors, and we will describe a few methods used to produce a document vector representation.

## Continuous-Bag-Of-Word representation

A simple method consists in representing a document by averaging the vector representations of the words producing the document. The words of the document may be weighted (for example by a tf-idf method).

## Other method

A document can also be represented by the concatenation of the words representations : the problem is that it leads to a very high-dimensional space, and the dimension is not fixed.

## Using a recurrent-neural network to represent a document

In a recurrent neural network, we have a recursive function which takes as input a state s(i) and the input word x(i+1), and it returns a state s(i+1).
Then, a function maps s with the output y.

A document is represented by the state s(i) after having read the whole document.

A problem made for building document embeddings may be for example to try to predict the word (i+1) of the document depending on words (1:i).

We have described here the use of a simple recurrent neural network, but in practice more advanced versions are more widely used, as LSTM or Gated Recurrent Unit networks, which enable to keep some information in memory.


# Conclusion

The idea here was to write a simple overview of some NLP methods. It was obviously not exhaustive at all. For me, the main thing missing here is that we do not talk about approaches which take into account the structure of the sentence.

Two things :
* PoS tagging features can be added into the model
* Using recursive neural networks, which model directly the tree-structure of a sentence

These two questions discard some kinds of models which will not be mentioned here, especially character-based models (see Karpathy) based on a language model character by character.

Example of a tranlator ? encoder qui encode le texte à traduire, decoder qui prédit les mots dans la nouvelle langue.

# TO ERASE

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
