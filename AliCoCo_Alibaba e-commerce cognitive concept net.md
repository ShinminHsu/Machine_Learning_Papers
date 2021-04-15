Luo, X., Liu, L., Yang, Y., Bo, L., Cao, Y., Wu, J., ... & Zhu, K. Q. (2020, June). AliCoCo: Alibaba e-commerce cognitive concept net. In _Proceedings of the 2020 ACM SIGMOD International Conference on Management of Data_ (pp. 313-327).

# 5. E-Commerce Concepts
## 5.2 Generation
### 5.2.1 Candidate Generation
1. Mining raw concepts from texts
	- search queries
	- product titles
	- user-written reviews
	- shopping guidance
2. Generating new candidates using existing primitive concepts
![[Alicoco_table1.png]]

### 5.2.2 Classification
- Goal: Automatically judge whether a candidate concept is a good e-commerce concept
- Method: ==Knowledge-enhanced deep classification model==
	- link each word of a concept to an external knowledge base
	- introduce rich semantic information from it
	- based on [[Wide_Deep_Learning_for_Recommender_Systems]]
	- input: a candidate concept c
	- output: score -> measuring the probability of c being good e-commerce concept
- Conduct word segmentation before feeding to the model
#### Knowledge-enhanced Deep Classification Model
![[Alicoco_Figure5.png]]
##### Deep
Two components
1. A char level BiLSTM
	- encode the candidate concept c by feeding the char-level embedding sequence
	- mean pooling
	- Output: $c_2$

2. Knowledge-enhanced module
	**Part 1**
	- Inputs: concatenate three embeddings
		a. pretrained word embeddings
		b. POS tag
		c. NER label
	- BiLSTM
	- Self Attention
	- Output: ${w'_1,w'_2,...,w'_m}$

	**Part 2**
	- Link each word to its corresponding Wikipedia article is possible
	- Extract the gloss of each article as the external knowledge -> Doc2vec to encode
	- Gloss: a short document to briefly introduce a word
	- Output: ${k'_1,k'_2,...,k'_m}$
	
	**Combine two parts**
	- Max-pooling
	- Output: $c_2$

##### Wide
- Adopt pre-calculated features
	- the number of characters and words of candidate concept
	- the perplexity of candidate concept calculated by a BERT model trained on e-commerce corpus
	- the popularity of each word appearing in e-commerce scenario
- Two fully connecteed layers
- Output: $c_3$

##### Final Score
- Concatenate the three embedding -> a MLP layer
- Point-wise learning with the negative log-likelihood objective function
![[Alicoco_Eq3.png|300]]

## 5.3 Understanding
- Goal: link e-commerce concepts to the primitve concepts -> `e-commerce concept tagging`
- Method: `a short text Named Entity Recognition (NER) problem`
- Challenges
	- short concept phrases
	- lack of contextual information
- Solution: a text-augmented deep NER model with fuzzy CRF
![[Alicoco_Figure6.png]]
- Output: In/Out/Begin (I/O/B)
### 5.3.1 Text-augmented concept encoder
Three features: word-level, char-level features and position features
- Char-level
	- Use CNN with window size k to extract the char-level features for each word
	- Max pooling
- Word-level
	- Use pre-trained word embeddings from GloVe
- Position features
	- Calculate part-of-speech tagging
The word representation wi = [xi; ci; pi]
-> feed into BiLSTM
-> get $h_i$

**Textual embedding matrix TM**
- Mapping each word back to large-scale text corpus to extract surrounding contexts and encode them via Doc2vec
- get $tm_i$

### 5.3.2 Fuzzy CRF layer
- 