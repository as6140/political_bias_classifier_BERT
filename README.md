# Exploring Bias in Tweets by Members of Congress
Mando Iwanaga & Alex Shropshire

[Exploring Bias in Tweets by Members of Congress](exploring_bias_tweets_by_congress.pdf)

## Business Understanding
Twitter has become an epicenter of social and political discourse within 280 characters, and it seems every member of Congress must now engage supporters and critics on the platform as a central part of their political communication strategy. These tweets are not only lacking in communicative depth and breadth, but are very prone to unproductive messaging and political bias. We hope to utilize the power of data science to explore such bias by Congressperson and be able to accurately classify bias as Democrat-leaning, Republican-leaning, or Neutral given new tweet text.

## Data Understanding
We found a great labelled dataset on Figure-Eight.com from 2015, spanning the 113th and 114th Congress members. Though we recognize that certain words are prone to swings in sentiment over time (ex. "president", "economy", etc.), we decided to employ this dataset to train our classifier. 

## Data Preparation
Labels existed as "partisan" or "neutral", but we manually labelled each partisan row as either Republican or Democrat based on the Congressperson's stated affiliation. We decided to drop the handful of tweets included that were written by non-affiliated Independent congresspeople. 

Tweets tend to be written in a variety of complicated, somewhat cryptic ways. There is slang, shrothand, abbreviations, domain-specific punctuation, hyperlinks, and a collection of meaningful and meaningless symbols. It was important for us to clean this up and get down to the core meaning of each word used.

## Modeling
We utilized a refactored version of Separius/BERT-keras with guidance from khumbuai/BERT-keras-minimal which focuses on providing the two functionalities:*
 #### Pretrained BERT model:
 [BERT-Base, Multilingual Cased (New, recommended)](https://storage.googleapis.com/bert_models/2018_11_23/multi_cased_L-12_H-768_A-12.zip): 104 languages, 12-layer, 768-hidden, 12-heads, 110M parameters
* Load a pretrained tensorflow BERT model (load_pretrained_bert.py) and use it as a usual keras model
* Provide a batch_generator (batch_generator.py) which provides the required input for the keras model (text tokenization, positional encoding and segment encoding).
* We keep the vocabulary ordering of the original tensoflow implementation.

 #### Loading google BERT from a tensorflow model:**
load_google_bert loads the weights from a pretrained tensorflow checkpoint. In particular, it loads all position embeddings weights, even if the keras model has a shorter sequence length. It is therefore possible to train a model with e.g. a sequence length of 70, with positional embeddings frozen and then to load the model for inference with a larger sequence length.

After extracting the valuable information yielded by BERT, we created a final Dense softmax layer of size 3 to classify each tweet (R, D, Neutral). We used a 75/25 train/test split, and had nearly 4K tweets to train on and just ovoer 1K tweets to test on, yielding a final model.


## Evaluation
The model was evaluated on the ~1K test tweets. The model had an accuracy of about 73%. We feel this score could be improved upon in a few different ways. Ideally, we'd use data that spanned a larger amount of time, we'd use a bigger data. We were also limited by Twitter's character limit and domain-specific use of punctuation and grammar. Training a model on political essays would surely provide more depth. Beyond this, without the inclusion of very strong, biased diction, our model would tend to classify tweets as neutral. We figured out a way to control the predicted probability thresholds, giving us a lever to pull to increase or decrease the model's responsiveness to words that may only slightly hint in a direction of bias.


## Deployment & Future Work
With more time to collaborate, we would hope to stand up a Flask app that allows users to view Congresspeople ranked by neutrality to view who keeps their political Twitter communication constructive and factual. We would also want to learn about those who are often biased in their Tweets, what words they like to use that may be leading to such a classification, and if these words are truly representative of a close-minded vocabulary or if they are simply victim to our model's limited knowledge. We'd also want to understand which specific buzzwords tend to swing classifications to one side or the other, and which ones are powerfully neutral. Please share any thoughts and ideas with us as you explore our process!

## Citations:
[BERT research](https://arxiv.org/abs/1810.04805)

[Figure Eight Data Source](https://www.figure-eight.com/data-for-everyone/)

[Khumbuai Example with Quora Data](https://github.com/khumbuai/BERT-keras-minimal/blob/master/example_kaggle_quora_insincere.py)

