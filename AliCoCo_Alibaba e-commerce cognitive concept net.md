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
#### Knowledge-enhanced deep classification model
##### Deep
Two components
1. A char level BiLSTM
	- encode the candidate concept c by feeding the char-level embedding sequence
	- mean pooling
	- get embedding $c_1$
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
	
	Combine two parts

##### Wide