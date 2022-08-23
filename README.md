# Stack_Overflow-Tag-Predictor

Problem: Given the Title and body of a question on Stack overflow, predict the tags associated with the question.

# Business Context

Stack Overflow is the largest, most trusted online community for developers
to learn, share their programming knowledge, and build their careers.
It is something which every programmer use one way or another. Each
month, over 50 million developers come to Stack Overflow to learn, share
their knowledge, and build their careers. It features questions and answers
on a wide range of topics in computer programming. The website serves as
a platform for users to ask and answer questions, and, through membership
and active participation, to vote questions and answers up or down and edit
questions and answers in a fashion similar to a wiki or Digg. As of April
2014 Stack Overflow has over 4,000,000 registered users, and it exceeded
10,000,000 questions in late August 2015. Based on the type of tags
assigned to questions, the top eight most discussed topics on the site are:
Java, JavaScript, C#, PHP, Android, jQuery, Python and HTML.

https://stackoverflow.com/.



# Problem Statement

The problem says that we will be provided with a bunch of questions. A question in Stack Overflow contains three segments Title, Description and
Tags. By using the text in the title and description we should suggest the
tags related to the subject of the question automatically. These tags are
extremely important for the proper working of Stack Overflow.
People can provide the tags related to the question on their own or Stack
Overflow can predict the tags using the text in title and description. This is
extremely business critical. The more accurately Stack Overflow can predict
these tags the better it can create an Ecosystem to send the right question to
the right set of people.

![image](https://user-images.githubusercontent.com/33951358/186154943-b408d508-0ace-4645-9d5d-d97e75471798.png)


# Objective 

1. Predict as many tags as possible with high precision and recall.
2. Incorrect tags could impact customer experience on Stack Overflow
3. No Strict Latency Constraints


# Data Overview

All of the data is in 2 files, Train and Test.
i. Train.csv contains 4 columns: Id,Title,Body,Tags.


ii. Test.csv contains the same columns but without the Tags, which we have
to predict.

iii. Size of Train.csv: 6.75GB

iv. Size of Test.csv: 2GB

v. Number of rows in Train.csv = 6034195


Data set contains 6,034,195 rows. The columns in the table are:
Id : Unique identifier for each question

Title: The question’s title

Body: The body of the question

Tags: The tags associated with the question in a space separated format (all
lowercase, should not contain tabs ‘\t’ or ampersands ‘&’)


# Solution Developed

This is a multilabel classification since a data sample can belongs to
more than one labels( A question can have more than one tags)

Performance Metrics : micro F1 Score ( Since precision and Recall
both must be high)

Micro F1 Score perform well when frequency of labels differs.

Step by Step procedure :
1. Remove Duplicate Data

2. EDA (Exploratory Data Analysis) : We Observe the behavior of tags
using visualization Techniques

![download](https://user-images.githubusercontent.com/33951358/186156500-f3112ff5-4822-464d-a5ad-3118eaebc72f.png)


We find out that we can use 500 frequent tags since it covers 90% of
questions
OR
We can use 5500 tags since it covers 99% of Question.


Note: We will be using One vs rest Classifier so we will need to train many binary classifier model so we must choose no of tags based on our memory and computing power.

3. Data Preprocessing
       Sample 100000 data points

       Separate out code-snippets from Body

       Remove Special characters from Question title and description (not in code)

       Remove stop words (Except 'C')

       Remove HTML Tags

       Convert all the characters into small letters

       Use SnowballStemmer to stem the words

4. Converting Target labels (y) into multi label problems (Converting tags into binary vectors using CountVectorizer)

5. Splitting the Data into 80:20 ratio

6. Using TF-idf Approach to convert input text (Preprocessed text into
vectors)

Note: using TF-IDF ,dimension increases . So for large input samples it might take a lot of computing and memory power.

7. Applying Logistic Regression with OneVsRest Classifier (for tfidf vectorizers)


##### Result 

accuracy : 0.0779

macro f1 score : 0.07769586628898778

micro f1 scoore : 0.37281303131684307

hamming loss : 0.00041583636363636364


##### Since micro f1 score is low ,we try to improve our solution using some hacks.


# Improvement to the solutions 

##### using 500 tags instead of 5500

##### using 50,000 points instead of 1,00,000

##### giving more Weightage to title (3*Title)

Rest all steps will be same for this improvement
 
 
3.Data Preprocessing
  
     Sample 50,000 data points

     Separate out code-snippets from Body

     Remove Special characters from Question title and description (not
     in code)

     Remove stop words (Except 'C')

     Remove HTML Tags

     Convert all the characters into small letters

     Use SnowballStemmer to stem the words


4. Converting Target labels (y) into multi label problems (Converting
tags into binary vectors using CountVectorizer)


5. Splitting the Data into 80:20 ratio

6. Using TF-idf Approach to convert input text (Preprocessed text into
vectors) Or We can use BOW

7. Applying Logistic Regression with OneVsRest Classifier (for tfidf
vectorizers) Vs  SVM with OneVsRest Classifier (for tfidf vectorizers)



##### Result


Logistic Regression with OneVsRest Classifier (Loss - Log & L1 Reg)

accuracy : 0.2371
macro f1 score : 0.3505517681526346

micro f1 scoore : 0.4800174710635511


SVM with OneVsRest Classifier (Loss - Hinge & L1 reg)

Accuracy : 0.2481

Hamming loss  0.0027434

Micro-average quality numbers
Precision: 0.7464, 
Recall: 0.3682, 
F1-measure: 0.4931

Macro-average quality numbers
Precision: 0.4728,
Recall: 0.2774,
F1-measure: 0.3242


# Summary

![Screenshot 2022-08-23 180704](https://user-images.githubusercontent.com/33951358/186159797-cdf6f2ed-dedd-4eaf-b15e-2a45eb393b9a.png)




# Observation :
1. We Observe that using Hack(Improvement) F1 Micro score for our
model improved.

2. Logistic Regression is preferred over other algorithms due to the
reason that its training time is less when dimension is large for our
dataset.








