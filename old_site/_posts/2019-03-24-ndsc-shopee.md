---
title: National Data Science Competition 2019 - Shopee
tags:
  - Data Science
  - NLP
  - CNN
---

## Objective

To build a predictive model for extracting the category of  e-commerce products automatically given their images and titles, as illustrated in the figure below.


<img src="{{ site.url }}{{ site.baseurl }}/images/NDSC/ndsc_data.PNG" alt="Problem definition">
<sub>*Fig 1: Problem definition: Product category classification.* </sub>

## Exploratory Data Analysis (EDA)

We first noted the following aspects of the dataset. 

* There are a total of 58 categories belonging to three top classes: beauty, fashion, and mobile. The number of items within the top categories are unbalanced, with approximately 43%, 33% and 24% of the products belonging to beauty, fashion, and mobile respectively in both train and test set.

* The  class distribution within the top categories are also unbalanced, e.g. beauty category contains 17 classes, with as high as 28% of the items are classified as powder, whereas only 0.2% are lip-liner. 

* The mobile category has distinct keywords (e.g. iPhone, Samsung, Nokia). In contrast, classification of beauty and fashion products are more challenging as it entail several  overlapping words e.g. cream in the most frequent word in both of the 'Other Face Cosmetics' and 'BB \& CC Cream' categories. Similarly, lengan, wanita, and dress are common words in fashion. 

* It is noted that in the beauty set, the distribution of categories is vastly different in training and test data. Test set contains significantly large number of items from categories 12-16.

* The items in the beauty and fashion are shuffled in a specific way. For example, top segment of the beauty dataset contains only categories from 0-11, whereas bottom segment constitutes of categories  12-16. This is true for both train and test set.

## Algorithm

For the current competition, we have adopted an ensembling (stacking) approach, where a gradient boosting (XGBoost) technique is used to combine information from multiple predictive models at the base level (level 0). The model architecture is illustrated in Fig 2. In general, the stacked model outperforms each of the individual models due to the inherent smoothing nature and ability to incorporate  strengths of the base models, while removing their biases. It is thus necessary to have significantly different base predictors for extracting diverse information from the dataset. To this end, we have built three models, each characterizing different aspects of the training set.

<img src="{{ site.url }}{{ site.baseurl }}/images/NDSC/Algorithm.PNG" alt="Algo">
<sub>*Fig 2: Model Architecture for Product Category Classification.* </sub>

### Image Data

The Convolutional Neural Networks (CNN) are widely used for image classification. A powerful image classifier can be built by (a) training a  CNN from scratch, (b) using the bottleneck features of a pre-trained network (outputs from penultimate layer), (c) fine-tuning the top layers of a pre-trained network. Building a CNN from scratch requires large amount of data. This constraint can be relaxed by adopting the later two approaches, which leverage on an established network pre-trained on a large dataset (e.g. imagenet). Even though fine-tuning, in general, produces better results, it can be computationally expensive. Due to limited computational resources, we thus adopted approach (b). Therewith, each image is first preprocessed and resized to dimension (128,128,3), and subsequently passed through the pre-trained MobileNet. The activations from the penultimate layer (bottleneck features) are forwarded to a Global Average Pooling2D layer and the resulting outputs are stored in memory. We thus encode each image by 1024 dimensional vector. Note that this encoding needs to done only once, thereby reducing computational cost. We chose MobileNet for this dataset, as its performance has been found out to be on par with VGG-16 and InceptionNet v3, while having significantly lesser number of parameters.

A deep neural network is build with two hidden layers, which takes image encoding as input and predicts probability of the image to belong to a particular category. It has been observed that adding Batch Normalization step for each hidden layer gives significant performance boost. 

### Text Data

#### LSTM and word2vec

The title descriptions must be preprocessed into a suitable numerical data structure before feeding into a machine learning algorithm. To achieve this goal, each relevant word in the text corpus can be mapped to a unique point in a finite dimensional vector space. Since the title text is in Bahasa Indonesia, we oped to design a custom word embedding matrix using word2vec model in Gensim. We thus can capture the interactions between words in a meaningful way e.g. the cosign similarity between `dress' and `cloth' is higher  than  `dress' and `iPhone'. 

The maximum word length of the title feature is determined to be 32. Title word sequences are next mapped into $(32,300)$ dimensions, with zero padding where necessary. The sequence acts as input to a LSTM recurrent neural network. The LSTM based model is found out to be a strong predictor for the product category. 

#### TF-iDF and SVD

An alternative way for converting texts to a numerical structure is TF-iDF based vectorization. TF-iDF measures the number of times that words appear in a given title, normalized with respect to its document frequency. It offers diversity into the base predictors, from which the meta learner (XGBoost) can extract additional information. We next performed  SDV operation on the TF-iDF matrix to retain useful information using much lesser dimensions. We designed and trained a Neural Network for TF-iDF + SDV based classification. 

### XGBoost

The class probabilities predicted from the base classifiers are concatenated and supplied as input to the XGBoost model. The XGBoost model helps to build a strong classifier by combining strengths of each base models.

## Validation

A proper validation strategy is of utmost importance to reconcile the bias-variance problem and determine the expected performance on the test set. Herewith, we have followed the sequence: 
* Create a holdout set comprising of  5% of train data, 
* Make *k* folds of the remaining train segment, 
* Fit a base  model on *k-1* folds and predict the *k*<sup>th</sup> fold, 
* Repeat the previous step for each fold,  
* Predict the probabilities untouched holdout and test set with each model trained on *k-1* folds and take average, 
* Put together out of fold predictions  and split it into train and validation set for the  XGBoost model, 
* Train the XGBoost model and measure its performance on holdout set, 
* Make the final prediction using XGBoost model.

<img src="{{ site.url }}{{ site.baseurl }}/images/NDSC/Valid0.PNG" alt="Valid">
<sub>*Fig 3: The validation strategy for product classification: Base Layer.* </sub>

<img src="{{ site.url }}{{ site.baseurl }}/images/NDSC/Valid1.PNG" alt="Valid1">
<sub>*Fig 4: The validation strategy for product classification: Ensemble Layer.* </sub>

## Discussion

The performance obtained from different models for different top categories are summarized  in Table 1. As observed, title descriptions are stronger predictor of product categories than image data. Notably, the mobile model performs better as the underlying classes have their own unique identifiers. A significant performance gain is achieved in fashion category via XGBoost.

<img src="{{ site.url }}{{ site.baseurl }}/images/NDSC/Table1.PNG" alt="table">

## Outlook
We have identified a few approaches to improve the model performance. For example, it is noted that in the beauty set, the distribution of categories is vastly different in training and test data. Test set contains significantly large number of items from categories 12-16. The oversampling or differential weighing of errors technique could have been implemented. We have developed an automatic image captioning framework (based on train + test data) to provide additional model features. However, due to limited time, we were unable to integrate it in the final model. This approach has potential to capture the interactions between image and title data. Finally, tuning of model hyperparameters can give a significant boost. 

## Acknowledgment

We would like to thank *Shopee* and other partners for conducting such an amazing event. It has been an enriching experience with a steep learning curve. The Kaggle and Medium community provide an ideal atmosphere for learning and practicing data science.  





















































