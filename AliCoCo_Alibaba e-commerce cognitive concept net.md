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
- Knowledge-enhanced deep classification model
	- link each word of a concept to an external knowledge base
	- introduce rich semantic information from it
	- based on [[Wide_Deep_Learning_for_Recommender_Systems]]
	- input: a candidate concept c
	- output: score -> measuring the probability of c being good e-commerce concept
- Conduct word segmentation before feeding to the model
#### Deep
Two components
- a char level BiLSTM
	- encode the candidate concept c by feeding the char-level embedding sequence
- 
