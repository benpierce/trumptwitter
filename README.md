# TrumpTwitter Bot 1.0

*Ben Pierce  |  SCS3546 (Deep Learning)  |  University of Toronto  |    Semester 1, 2020*

**Data Source**: Donald Trump's Twitter feed (http://www.trumptwitterarchive.com/archive)

This project is first and foremost a text generation learning project using deep neural networks. The goal of the project is to ingest a significant number of tweets (in this case Donald Trump's twitter feed) and to be able to produce new tweets in a similar format and on similar themes.

Why is something like this significant? Imagine being able to feed a neural net a cross section of tweets based on timeseries, geographic location or another demographically significant attribute. With a good enough neural network, topics could be provided to the neural net and opinions or sentiments could be mined and studied without having to interact with real individuals (typically the most cost-intensive part of a research study).

---

For this exercise, we're going to attempt to train a neural net in two different ways to see which works best: learning by words and learning by characters. The TweetProcessor constructor, by default, will learn by words. To learn by individual characters, simply pass method='bychar' to the constructor.

To experiment on the training/validation side of things, we're also going to allow the training/vaidation sets to be produced in two possible ways: rolling sequences and padded sequences. The type of sequences can be specified via the *sequence_type* parameter of the *get_train_valid_set* method.

**Rolling Sequences** 

Rolling sequences can be thought of as a sliding window across a series of text and is going to be the most RAM efficient way of preparing training/validation data. If we take the sample phrase "*Proud to welcome our great Cabinet this afternoon for our first meeting*" and convert it to a set of training data with a sequence length of 5 words, we'll end up with the following results:

Sequence1 | Sequence2 | Sequence3 | Sequence4 | Sequence5 | Label
--- | --- | --- | --- | --- | ---
Proud | to | welcome | our | great | Cabinet
to | welcome | our | great | Cabinet | this
welcome | our | great | Cabinet | this | afternoon
our | great | Cabinet | this | afternoon | for
great | Cabinet | this | afternoon | for | our
Cabinet | this | afternoon | for | our | first
this | afternoon | for | our | first | meeting

**Padded Sequences**

Padded sequences can be thought of as two sliding windows across a series of text: the first window will be across each word or character and another sliding window for n to sequence length. This means that there is going to be a lot of additional training data produced from each series of text and it's going to require a lot more additional RAM. For the phrase "*Proud to welcome our great Cabinet this afternoon for our first meeting*" we ended up with 7 samples in our training set; however, for padded sequences we're going to get 49 samples! An abbreviated result looks like this (note, in actuality the words are tokenized into floats):

Sequence1 | Sequence2 | Sequence3 | Sequence4 | Sequence5 | Label
--- | --- | --- | --- | --- | ---
0 | 0 | 0 | 0 | Proud | to
0 | 0 | 0 | Proud | to | welcome
0 | 0 | Proud | to | welcome | our
0 | Proud | to | welcome | our | great
Proud | to | welcome | our | great | Cabinet
0 | 0 | 0 | 0 | to | welcome
0 | 0 | 0 | to | welcome | our
0 | 0 | to | welcome | our | great
0 | to | welcome | our | great | Cabinet
to | welcome | our | great | Cabinet | this
... | ... | ... | ... | ... | ...
0 | 0 | 0 | 0 | our | first
0 | 0 | 0 | our | first | meeting
0 | 0 | 0 | 0 | first | meeting



---

# Conclusion

Padded sequences outperformed rolling sequences in both the word-based and character-based models.

Despite the lower validation accuracy for word-based models compared to character-based models, world-base models generated more coherent tweets due to the fact that the model was predicting known words rather than character sequences, the latter of which could result in non-sensical words that don't exist in the English vocabulary.

Disappointingly, many of the generated tweets weren't very good and many of them are incoherent. There are a few explainations for this:

* Due to Twitter's character limits, the training text has a lot of grammatical errors to begin with. This is further compounded by hashtags, uri's, and other atypical text that makes it very difficult for a model to be trained. 
* Much more training data could have been used, but Google Colab doesn't allow Canadians to pay for increased capacity, therefore we're stuck using their basic virtual environments and can only load a small subset of data for training.
* Google Colab's basic virtual enviroments are also not very stable. Training for a long period of time, over a longer number of epochs may have helped, but Colab would frequently crash (especially if left to run for more than an hour).

I believe that with a more powerful and more stable environment, **a word-based, padded-sequence**, model run against the entire volume of training data would have produced very realistic tweets and that more work can be done in this area if and when Google Colab resource limits are increased.
