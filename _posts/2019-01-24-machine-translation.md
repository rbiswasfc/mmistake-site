---
title: Neural Machine Translation using Encoder-Decoder Networks
tags:
  - NLP
  - PyTorch
---


## Neural Machine Translation (NMT) 
#### -- Project Status: Active

In this project, I aim at building different sequence to sequence machine learning models for Spanish to English translation task using PyTorch. The Sequence to sequence or "Seq2Seq"  is an end-to-end model comprising of two recurrent neural networks:
* An Encoder: takes sentences in the source language as input and encodes them into finite dimensional context vectors;
* A Decoder: uses the context vector as a seed from which the translated sentences are generated.    

For the NMT task, I have adopted four different Seq2Seq architectures:
* LSTM Encoder-Decoder 
* LSTM Encoder-Decoder with attention
* Sub-word modelling with character level CNN
* The Transformer model

Brief explanations of each approaches are provided in the following.



### [LSTM Encoder-Decoder Network](https://github.com/rbiswasfc/Machine-Translation/blob/master/Vanilla-LSTM/LSTM-Encoder-Decoder-Vanilla.ipynb)
The architecture of LSTM Encoder-Decoder Network is illustrated below

![*The basic LSTM Encoder-Decoder Network.*](https://github.com/rbiswasfc/Machine-Translation/blob/master/images/Vanilla_LSTM.PNG)

A bi-directional LSTM is used as Encoder, since  a word in a sentence can have a dependency on another word before or after it. 
The encoder encapsulates information in the source sentence into a context vector, which acts as initial hidden state for the Decoder network.
The implementation of the model is elaborated in this [Jupyter Notebook](https://github.com/rbiswasfc/Machine-Translation/blob/master/Vanilla-LSTM/LSTM-Encoder-Decoder-Vanilla.ipynb).

#### Remarks:  
* It is difficult to compress an arbitrary-length source sequence into
a single fixed-size context vector. The issue can be mitigated by building Encoder with stacked LSTM layers: each layer’s outputs are the input sequence to
the next layer. 

* Seq2Seq models are known to lose effectiveness on very long inputs, a consequence of the practical limits of
LSTMs. Encoder-Decoder network with attention mechanism can help in capturing long term dependency.


### LSTM Encoder-Decoder with Attention
tbc

### [Sub-word modelling with character level CNN](https://github.com/rbiswasfc/Machine-Translation/blob/master/sub-word/Machine-Translation-Char-Based.ipynb)
tbc

### [The Transformer model](https://github.com/rbiswasfc/Machine-Translation/blob/master/the-transformer/TheTransformer.ipynb)
tbc

## Training

## Evaluation

## To-do list
* Train the transformer
* Investigate different initialization
* Build stacked LSTM encoder-decoder with attention
* Plot attention and demonstrate alignment during translation
* Learning rate scheduler
* Use the model architecture for other open-source machine translation datasets

## Credits
This project uses the public resources provided in [*CS224n: Natural Language Processing with Deep Learning*](http://web.stanford.edu/class/cs224n/) Stanford / Winter 2019 course. I am thankful to the lecturers and TAs for offering such amazing contents. I believe this course is one of the best places to learn NLP in 2019.

## Contact
* Raja Biswas: rajjjabiswas@gmail.com