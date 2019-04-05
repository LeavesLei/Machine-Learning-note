# Embedding
- word embedding
- sentence embedding
- multimodal embedding

## Word Embedding
### skip-gram model
#### GOAL
learn the probability of a context of words given a source word.
#### ARCHITECTURE
- Input layer：v×1 (v is the total number of words in text)
- Embedding layer：d×1
- Output layer(adding softmax): v×1 , which present the conditional probobility of a context of wordS given a source word

---

### CBOW model (continuous bag-of-words)
#### GOAL
learn the probability of a word given its context
#### ARCHITECTURE
CBOW's architecture is same as skip-gram but different train set so different function were presented.

---

### different features between skip-gram and CBOW
- Skip-gram works well with little training data and represents well rare words
- CBOW is faster to train and has slightly better accuracies for frequent words

---

### GloVe model (global vector)
#### co-occurrence probability matrix
- BLOG: [GloVe-CN-edition](https://zhuanlan.zhihu.com/p/42073620)

## Sentence Embedding
difficulty: sentences have different length
### bag of words - one-hot vector
un-ordered
![bow](https://github.com/LeavesLei/Machine-Learning-note/blob/master/image/bag-of-words.jpg)

### paragraph vector
![paragraph-vector](https://github.com/LeavesLei/Machine-Learning-note/blob/master/image/paragraph-vector.jpg)

### Skip-through

### SNLI - BiLSTM

## Multimodal Embedding
- multimodal embedding = image embedding + text embedding
![multimodal-embedding](https://github.com/LeavesLei/Machine-Learning-note/blob/master/image/multimodal-embedding.jpg)


