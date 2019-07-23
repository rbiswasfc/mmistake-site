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



### LSTM Encoder-Decoder Network
The purpose of this project is to implement Transformer architecture in Attention Is All You Need paper in PyTorch.

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
* Use the model architecture for othe open-source machine translation datas

## Credits
This project uses the public resorses provided in [*CS224n: Natural Language Processing with Deep Learning*](http://web.stanford.edu/class/cs224n/) Stanford / Winter 2019 course. I am thankful to the lecturers and TAs for offering such amazing contents. I believe this course is one of the best places to learn NLP in 2019.

## Contact
* Raja Biswas: rajjjabiswas@gmail.com