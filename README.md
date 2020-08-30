# scientific_article_recommender
Background	

In aim of the system is to allow the user to get article recommendations based on a given scientific paper. These recommendations should provide available scientific articles within the same or related domains as the given article, with topics and contexts linked to those of the main article. The main use case of this system is to allow an individual interested in a subject easily get more articles on a given topic.

Basic Design and Context

To achieve the described aim a Scientific Article Recommendation System is introduced. 
The system is constructed by means of the Apache Spark, a unified analytics engine for large-scale data processing. This engine has been chosen as it provides a set of tools needed for loading, analyzing and processing of data, while allowing for a scaling opportunity without the change of APIs or developed code.  Therefore, the throughput of the system can further be easily scaled up to match demand.

The Scientific Article Recommendation System consists of three main components: Data Base, Feature Extractor and Recommender.
•	The Data Base allows to store and retrieve the pool of all available scientific articles. It also allows to easily search and filter its entries based on provided conditions. All data base operations can be scaled by means of the underlying Apache Spark platform.
•	The Feature Extractor allows to transform the key information of scientific articles into numerical features, that can further be used for analysis of articles as well as finding best recommendations for the user.
•	The Recommender provides a way for the user to retrieve similar scientific articles based on a given article. It searches the available pool of articles to find those that strongly correlate with the given article. This is done based on the numerical features extracted by the Feature Extractor.
All interactions between the user and the system are intended to be performed through a user interface, the design of which is not covered by the purpose of this document and is understood to be remaining for future development.

Design Decisions
In this section we will explain the important design decisions we made when choosing the technologies and the general design principles of the system.
Language
Python is chosen as the main programming language for the Scientific Article Recommendation System(SARS). Python is a widely used programming language with a constantly growing community and support. A large variety of packages is available with Python through third party libraries. A Python API is available with the Apache Spark engine. This language also provides an easy way of prototyping by means of Jupyter Notebooks. All these factors have contributed to the choice of the main programming language. 
It is to be noted that Python is not by any means the most efficient and fast programming language available. Nevertheless, for processing large amounts of data, most tasks usually are not limited by the language optimization, but by the hardware limitations. Especially while using a strongly optimized engine for data manipulations, python serves a role of merely a high-level director of the processes. Therefore, possible disadvantages in performance are considered to be negligible for the given task.

Data processing engine
Apache Spark has been chosen as the data processing engine. Apache Spark provides generalized APIs to access data independently of how the data is stored. It also gives an easy way of scaling up the throughput of the data pipeline by simply increasing the number of worker nodes. 

Database 
The database of scientific articles is based on the publicly available data from the arxiv.org web site through the Kaggle platform. This database is loaded into the SARS from a CSV file and is kept in memory for the entire lifespan of the application. This was only possible due to its limited size and it is understood that in case of further expansion of the data available other solution such as an SQL server database would be a better option. As the Apache Spark provides a common API to a large variety of databases, changing from storing data in RAM to an external database server would not require significant changes to the developed system. In the current case, when the size of the database allows to easily place it in RAM in its entirety, it also provides a significant benefit in data access times. 

Feature extraction
In order to extract the meaning of each of the scientific articles in a numerical form, feature extraction step is needed. As a feature extractor TF-IDF(term frequency–inverse document frequency) was used. It is a numerical statistic that is intended to reflect how important a word is to a document in a collection or corpus. It is often used as a weighting factor in searches of information retrieval, text mining, and user modeling. The tf–idf value increases proportionally to the number of times a word appears in the document and is offset by the number of documents in the corpus that contain the word, which helps to adjust for the fact that some words appear more frequently in general. 

Prior to feature extraction, stop words are removed. Stop words are the most common words in a language, that do not influence the context of the text. In order not to consider these words while finding similar articles, they are filtered out. Our experiments have shown that adding this component helped to improve the quality of recommendations dramatically as it reduced the number of false recommendations that focused on presence of rare stop words in the titles of articles.

Recommender 
In order to find articles like the given one, keywords are extracted from the title of the given article. All other articles in the data base are ranked against keywords of the given article. Articles having the highest rank, are selected as similar to the given article.  

Architecture

The architecture of the system is given below. Initially all articles are processed by the stop words removal and feature extraction and stored in the DataBase. In order to find similar articles, an article passes through the same components of the system and is ranked against all existing articles from the Data Base. The articles with the highest rank are selected as recommendation results.


Evaluation
Evaluating similarity between context of two articles is in itself a complex NLP problem. This makes it hard to evaluate the performance of our recommendation system. Therefore, we propose a qualitative way to assess the quality of recommendations made by our system. 
For every recommendation it is checked whether the suggested articles are about the same scientific domain or a related domain to the one of the given articles. The recommended articles are checked in terms of relevance to the end user. The main question that should be answered is if the suggested articles might be interesting to read for a user that is interested in a certain given article. Assessing the power of this interest is beyond the scope of this project.
