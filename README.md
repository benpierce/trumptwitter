# TrumpTwitter Bot 1.0

*Ben Pierce  |  SCS_3546_007 (Deep Learning)  |  University of Toronto  |    Semester 1, 2020*

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

Padded sequences can be thought of as two sliding windows across a series of text: the first window will be across each word or character and another sliding window for n to sequence length. This means that there is going to be a lot of additional training data produced from each series of text and it's going to require a lot more additional RAM. For the phrase "*Proud to welcome our great Cabinet this afternoon for our first meeting*" we ended up with 7 samples in our training set; however, for padded sequences we're going to get 49 samples! An abbreviated result looks like this:

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
