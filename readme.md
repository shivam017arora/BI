# Business Intelligence Tool with Search Based Analytics

## Model Description

BERT-based model to solve the text-to-SQL problem: 

Content Enhanced BERT-based Text-to-SQL Generation https://arxiv.org/abs/1910.07179

## Brief Introduction

Semantic parsing is the tasks of translating natural language to logic form. Mapping from natural language to SQL (NL2SQL) is an important semantic parsing task for question answering system. In recent years, deep learning and BERT based model have shown significant improvement on this task. However, past methods did not encode the table content for the input of deep model. In order to solve the problem that the table content is not used for model, we propose our effective encoding methods, which could incorporate database designing information into the model. Our key contribution are three folds:
1. We use the match info of all the table cells and question string to mark the question and produce a feature vector which is the same length to the question. 
2. We use the match inf of all the table column name and question string to mark the column and produce a feature vector which is the same length to the table header.
3. We design the whole BERT-based model and take the two feature vector above as external inputs.

## Benchmark on WikiSQL: https://github.com/salesforce/WikiSQL

WikiSQL is a large semantic parsing dataset. It has 80654 natural language and corresponding SQL pairs. BERT is a very deep transformer-based model. It first pre-train on very large corpus using the mask language model loss and the next-sentence loss. And then we could fine-tune BERT on a variety of specific tasks like text classification, text matching and natural language inference and set new state-of-the-art performance on them.

## Deep Neural Model

We also use three sub-model to predict the SELECT part, AGG part and WHERE part. We use BERT as the representation layer. The question and table header are concat and then input to BERT, so that the question and table header have the attention interaction information of each other. 

1. SELECT COL: Goal is to predict the column name in the table header. The inputs are the question Q and table header H. The output are the probability of select column.
2. SELECT AGG: Goal is to predict agg slot. The inputs are Q with QV (external feature vectors) and the output are the probability of select agg.
3. WHERE NUMBER: Goal is to predict the where number slot. The inputs are Q and H with QV and HV (external feature vectors) and the output are the probability of where number.
4. WHERE COLUMN: Goal is to predict the where column slot for each condition of WHERE clause. The inputs are Q, H, and P(WN), probability of where number, QV and HV (external feature vectors) and the output are the top wherenumber probability of where column.
5. WHERE OP: Goal is to predict the where column slot for each condition of WHERE clause. The inputs are Q, H, P(wc), P(wn) and the ouput are the probability of where opration.
6. WHERE VALUE: Goal is to precict the where column slot for each condition of WHERE clause. The inputs are Q, H, P(wc), P(wn), P(wo), QV and HV and the output are the probability of where value. 

## Requirements

1. Python 3.6
2. Record 0.5.3
3. Torch 1.1.0

## Run

1. Data: ADD LINK

Download the data and put it at 'data_and_model' directory
```
Run data_and_model/output_entity.py
```
2. Train and Evaluate
```
Run train.py
```
## Data Example
```
{
	"table_id": "1-1000181-1",
	"phase": 1,
	"question": "Tell me what the notes are for South Australia ",
	"question_tok": ["Tell", "me", "what", "the", "notes", "are", "for", "South", "Australia"],
	"sql": {
		"sel": 5,
		"conds": [
			[3, 0, "SOUTH AUSTRALIA"]
		],
		"agg": 0
	},
	"query": {
		"sel": 5,
		"conds": [
			[3, 0, "SOUTH AUSTRALIA"]
		],
		"agg": 0
	},
	"wvi_corenlp": [
		[7, 8]
	],
	"bertindex_knowledge": [0, 0, 0, 0, 4, 0, 0, 1, 3],
	"header_knowledge": [2, 0, 0, 2, 0, 1]
}
```
## Trained Model

ADD LINK

