# QA_astroph
project module 4 CAS_NLP

Extractive QA system, project module 4 - CAS NLP

The approach is the following:
1- using abstracts from arXiv/astroph.EP (from 2009 until now) - already used in another project - we generate Q&A samples by this method:
	- split abstracts in 4 pieces 
	- use a T5-derived model to generate questions (model is from Huggingface: valhalla/t5-base-qa-qg-hl)
	- use deepset/roberta-base-squad2 to generate answer to these questions, using the piece of the abstract as context
	- this produced a set of ~300000 Q&A that will be used for fine-tuning
	- this part is coded in Creating_QA.ipynb
	- note that in the notebook, there is the code to directly download full papers from arXiv, and use this as a basis for Q&A. 
	- the abstracts were judged more relevant (they contain more 'useful' information)

2- using this set of Q&A to fine-tune bert-base-uncased
	- this model is used in its 'question-answering' version
	- it has to predict the starting and ending position of the tokens, in the context, that answer to the question (this is extractive Q&A, not generative Q&A)
	- the model is trained for 10 epochs, with batch size of 64. This takes a few hours on UBELIX (GPU 4090)

A few notes:
1-abstracts are split (train/eval/test) before generating questions, so there should be no data leadking
2- the quality of the automatically generated Q&A is not huge. For real application, it would be worth creating manually a sub-set of good quality questions (or at least removing the ones that are not good in the automatically generated dataset)

