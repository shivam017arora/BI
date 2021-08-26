# Business Intelligence Tool with Search Based Analytics

## What is Business Intelligence?

With the continual generation of phenomenal amounts of data, structured and unstructured, organizations are inundated with information from varied sources stashed away in numerous data storage systems. Business intelligence (BI) combines business analytics, data mining, data visualization, data tools and infrastructure, and best practices to help organizations to make more data-driven decisions. 

## Future of Business Intelligence

Conversational Analytics: is a Natural Language Processing (NLP)-driven analytics solution that incorporates AI/ML technologies to transliterate data directed through chatbots or voice assistants and enable an organization to leverage market opportunities through insights from its source of large datasets. The efficacy of conversational analytics depends upon its capability to access data from multiple sources and make use of it in real-time.

## Why is it better?

1. The system should offer not only descriptive analysis, but predictive analysis as well using Machine Learning. 
2. The system automatically presents important and meaningful insights to the user related to what they have searched on, without the user asking for a specific type of analysis using Machine Learning.
3. The system presents a narrative to the user (written/spoken) alongside the visualization that describes the key insight. 
4. The system returns a best-fit visualisation to the user wih the answer, and also allows the user to change and customise the visualisation. 
5. The system suggests search terms and phrases as the user makes their entry, guiding them to complete their question faster.
6. The system understands words that have similar meaning, such as "revenue" and "sales" and correctly give an answer regardless of which specific terminology is input.
7. The system can be taught new words and learn how to properly translate them as part of a query.
8. The system allows the user to clearly see the underlying query and data columns used to generate an answer. 
9. The system allows the user to combine visualisations into a dashboard and share them with other users. 
10. The system automatically organises the data to support the analysys that are run, even when new data is brought into the system.

## Types of Analysis

1. Aggregation and Group By: What were the total sales by region and department
2. Filtering: What were sales in California?
3. Top/Bottom: What were the top 10 selling brands last year?
4. Time Periods: What were sales last year?
5. Comparisons: What were the customer feedback of this year compared to last year?
6. Trends: What were the weekly revenue in California?
7. 'Why' analysis: Why did sales of electronics increase in Texas, last quarter?

## Problem

Text-to-SQL is a task to translate a user’s query spoken in natural language into SQL automatically.

Imagine a superstar recruiter, who can sell any job to anyone and also has extensive networks. But he has one drawback — he doesn’t know how to query the SQL database. He has experience and insights that he can draw from the data, but he just doesn’t have enough time to learn SQL. There are thousand different types of SQL queries that a user can ask, but we are focusing on solving the following cases only which we believe cover most of enterprise use cases:

1. One WHERE condition
2. Multiple WHERE conditions
3. With an aggregation
4. With JOIN, GROUP BY, HAVING, ORDER BY, etc.

The relational database was blamed for the lack of scalability about 10 years ago and NoSQL came into the market as a promising solution. However, we’ve learned over time that NoSQL can be a wrong tool for many application use cases. Also, the relational database has evolved to be able to scale horizontally and serve data with low latency while meeting availability requirements. If this problem is solved, it’s going to be widely useful because the vast majority of data in our lives is stored in relational databases. In fact, Healthcare, financial services, and sales industries exclusively use the relational database. This means the industries that can’t afford to lose transactions solely use the relational database, also writing SQL queries can be prohibitive to non-technical users.

## Challenges

1. The same questions will be spoken in so many different ways.

Solution is to use contextualised (language model pre-trained) word representation such as BERT. 

2. We need to match natural lanuage with database schema (column names)

Solution is to use attention, not only use a natural lnaugage query as an input, but also the column names of the tables as well and take advantage of attention mechanism (which helps the model focus on the query words or subwords that are most relevant to each label during the training)

3. We are not only predicting WHERE columns but also WHERE operators

Solution is to use execution-guided decoding. For example, you can't sum over a column with a string type also you can't apply > or < to a column of string type. Execution guided decoding detects and excludes faulty SQL during the decoding.


## Model Description

Refrence: https://github.com/guotong1988/NL2SQL-RULE

BERT-based model to solve the text-to-SQL problem: 

Content Enhanced BERT-based Text-to-SQL Generation https://arxiv.org/abs/1910.07179

## Brief Introduction

Semantic parsing is the tasks of translating natural language to logic form. Mapping from natural language to SQL (NL2SQL) is an important semantic parsing task for question answering system. In recent years, deep learning and BERT based model have shown significant improvement on this task. However, past methods did not encode the table content for the input of deep model. In order to solve the problem that the table content is not used for model, we propose our effective encoding methods, which could incorporate database designing information into the model. Our key contribution are three folds:
1. We use the match info of all the table cells and question string to mark the question and produce a feature vector which is the same length to the question. 
2. We use the match inf of all the table column name and question string to mark the column and produce a feature vector which is the same length to the table header.
3. We design the whole BERT-based model and take the two feature vector above as external inputs.

## Benchmark on WikiSQL

Link: https://github.com/salesforce/WikiSQL

WikiSQL is a large semantic parsing dataset. It has 80654 natural language and corresponding SQL pairs. BERT is a very deep transformer-based model. It first pre-train on very large corpus using the mask language model loss and the next-sentence loss. And then we could fine-tune BERT on a variety of specific tasks like text classification, text matching and natural language inference and set new state-of-the-art performance on them.

## WikiSQL Limitations

While WikiSQL is an exceptionally well-made dataset, it has one pitfall — it assumes NO JOIN. It takes for granted that a single table will have all the columns you need for a query. However, you will need a JOIN in real life because enterprise databases are normalized into subject-base tables. The number of columns in WikiSQL dataset is usually 5–7 and the number of rows is about 10. Meanwhile, a typical enterprise table has 30–40 columns and millions of rows in a single table.

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

1. Data: https://drive.google.com/drive/folders/16qfIBlkCY5JsEFYoDKzpAg0PPaE1_ad3?usp=sharing

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

https://drive.google.com/drive/folders/16qfIBlkCY5JsEFYoDKzpAg0PPaE1_ad3?usp=sharing

## Bottle-Necks

1. Research datasets assume the values in natural language questions (e.g. how much did we pay Zuck last month?) would appear exactly the same way (last month) in the database. In production, the natural language ‘last month’ is stored as ‘2020–02–01’ in the database, ‘10k’ will be ‘10,000’ in the database, and ‘NY’ will be ‘New York’ in the database. The model should know how to match these NL slangs with the values in DB.

2. Interdependencies between columns i.e For example, when ‘Employment Status’ is “Not active”, ‘Last Salary’ column should be returned, not the ‘Current Salary’.

3. Similar column names in enterprise databases i.e For example, “EstimatedRevenue” vs “ActualRevenue”, “CreatedOn” vs “ModifiedOn”, “CreatedOn” vs “CreatedBy”, ‘LastSalary’ vs ‘Salary’, etc.

## Future Scope

How do we solve the challenges of Text-to-SQL in production? 

1. Transfer Learning: it’s not the more complex model, it’s the training data that matters the most to the performance of the Deep Learning model. In other words, DL models are often constrained by the availability of large-scale high-quality training data.

2. CDM or Common Data Model: CDM is a standard & extensible schema (entities, attributes, relationships) that represents concepts that are frequently used in enterprises. CDM exists in order to facilitate data interoperability.

3. In production, we have access to the cell content of the database. (If we do this in research, it’s cheating.) We build an index on the cell values so that it can identify the values in the NL query and finding the most appropriate columns.

4. The key idea of execution-guided decoding is that a partially generated SQL can be executed and the results of that execution can be used to guide the rest of SQL generation. For example, the execution-guided decoder would eliminate an incorrect string-to-string inequality comparison “… WHERE department > ‘software engineering’ …” from the beam immediately after the token ‘software engineering’ is returned. Excluding erroneous candidates and not proceeding further during the beam search increases the accuracy of the model.
