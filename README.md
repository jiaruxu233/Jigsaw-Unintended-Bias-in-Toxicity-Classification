# DNSC 6290 Project Report -- [Jigsaw Unintended Bias in Toxicity Classification](https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification)

### Group 7: Abby Liu, Jiaru Xu, Yuqi Liu, Weiteng Li


![competition icon](https://github.com/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Pictures/Competition_Title.png)



## Background & Introduction

The Conversation AI team, a research initiative founded by Jigsaw and Google (both part of Alphabet), builds technology to protect voices in conversation. 

A main area of focus is **machine learning models that can identify toxicity in online conversations**, where toxicity is defined as anything rude, disrespectful or otherwise likely to make someone leave a discussion.

When the Conversation AI team first built toxicity models, **they found that models predicted a high likelihood of toxicity for comments containing those identities (e.g. "gay"), even when those comments were not actually toxic (such as "I am a gay woman")**. This happens because training data was pulled from available sources where unfortunately, certain identities are overwhelmingly referred to in offensive ways. Training a model from data with these imbalances risks simply mirroring those biases back to users.
<br/>
<br/>

### What is Unintended Bias? What is the problem? 

Here's a good example from training set that clarifies the main problem we face in the competition.

![why hard](https://github.com/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Pictures/why_hard.png)

For the Toxity Comment classification task, **the model is prone to predict the last comment to toxic class**,because it mentions 'gay'.

To solve this problem, we need to first come up with a metric that can estimate  whether the model do a good job for this type of problem. Therefore, here comes the competition evaluation metric.
<br/>

### Evaluation Metric

Basically, the finally score is an average of 4 AUC. 3 of them only take into account parts of the dataset that depending on whether the comment mentions word like 'gay' and whether it's toxic.

![metric](https://github.com/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Pictures/metric.png)

![metric2](https://github.com/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Pictures/metric2.png)

**Score = 1/4 OverAll_AUC + 1/4 Subgrou_AUC + 1/4 BPSN_AUC + 1/4 BNSP_AUC**

For more detail explanation, please check the link [here](https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/overview/evaluation).




Here's an prediction validation score of one of our model. We can see that the subgroup AUC is pretty low, which affects the overall score a lot.


![metric3](https://github.com/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Pictures/metric3.png)




## What we did & What we learned

### [Part 1  A Good Teacher Is Helpful For All Students -- Custom Loss](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Custom_Loss.ipynb)

Because of the 4 AUC average evaluation metric, we try to make a custom loss fuction instead of just using the binary cross entropy. It turn out that not only the custom loss fuction works well (boost LSTM model AUC:0.930 -> 0.934 when doing experiment), when we use the custom loss for fine-tuning Bert & GPT2, it also gives the model great boost.
<br/>

### [Part 2  Flexibility Make the Life Much More Easier -- LSTM Playground](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/LSTM_Playground.ipynb) 

Because we don't have enough computation power (most of the time we only use kaggle kernel with single Tesla P100 GPU), it's hard for us to do even sigle epoch fune-tuning for Bert & GPT2.
But there's still a lot of meaningful and interesting work to do with LSTM, which later helps a lot when we try to do fine-tuning.
<br/>

###  [Part 3  Harness the Beast -- Fine-Tuning Google Bert & OpenAI GPT2](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Harness_the_Beast.ipynb) 

Transfer learning is one of the most important method to train a state-of-the-art NLP model after the [Ulmfit Paper](https://arxiv.org/abs/1801.06146) & [Google Bert](https://github.com/google-research/bert) came out.
Fine-tuning Bert & GPT2 requires huge computation power, but the conclusion is -- it totally worth it.

NLP is develping rapidly!  Single Bert Base Model without any preprocessing or custom loss can easily reach a relatively high score, let alone great a mount of paper about advanced fine-tuning skill. 

For us, plug and play fine-tuning Bert can slightly outperform an elaborate LSTM model. Then, we do a bunch of work to improve the Bert and copy and paste the same approach to fine-tuning a OpenAI GPT2 Model.

Here's the AUC comparison of different model:

|Model|AUC|
|---|---|
|Plug and Play Bert|0.9367|
|Ensemble 6 different LSTM|0.9368|
|Best Bert|0.9415|
|Best GPT2|0.9388|


### [Part 4  Ensemble Is All You Need -- Blending To Achieve Higher Score](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Final_Blending.ipynb)

Ensemble is a very powerful machine learning techique, we did't spend a lot of time to do this, but simply blending all of the model can give us great boost. (about 0.003 boost from single best model)

### For detail explanation, please check the link here:

* [ Part 1: A Good Teacher Is Helpful For All Students -- Custom Loss](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Custom_Loss.ipynb) 
* [ Part 2: Flexibility Make the Life Much More Easier -- LSTM Playground](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/LSTM_Playground.ipynb) 
* [ Part 3: Harness the Beast -- Fine-Tuning Google Bert & OpenAI GPT2](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Harness_the_Beast.ipynb) 
* [ Part 4: Ensembling Is All You Need -- Blending To Achieve Higher Score](https://nbviewer.jupyter.org/github/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Final_Blending.ipynb)
<br/>
<br/>

##  Conclutions & Result
All in all, it's an interesting competition. We learned a lot from other kagglers. We realize that all the model is useful for some aspect. LSTM is good for doing experiment, Bert & GPT2 is really powerful and accurate. 

Finally our reach about 0.9446 on the LB, which ranks about 75.

![score 1](https://github.com/jiaruxu233/Jigsaw-Toxicity-Classification/blob/master/Pictures/Score.png)

![score 2](https://github.com/jiaruxu233/2019summer-Machine-Learning-Project/blob/master/Pictures/groupscore.png)
