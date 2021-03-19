This project aims at identifying threats and addressing them by defining experiments, implementing solutions and deriving conclusions on the paper "Can issues reported at Stack Overflow questions be reproduced? An exploratory study".  

# Baseline
# Meta data
1. A reproduction as part of MSR course at MSR course 2020/21 at UniKo, CS Department, SoftLang team.
2. Paper title: "Can issues reported at Stack Overflow questions be reproduced? An exploratory study"
[DBLB link](https://dblp.org/rec/conf/msr/Mondal0R19.html?view=bibtex)
# Requirements
1. Hardware requirements : 
> * 4GB RAM or more
> * Any version of Windows 10 opearating system
2. Software requirements : 
> * Microsoft .NET 3.5 Framework or higher
> * MySQL workbench
> * Updated Java SE Development Kit
> * Eclipse IDE 2020/12 
> * NetBeans IDE
> * Anaconda  
# Process
**1. Data extraction** 
> * Navigate to the Stack Overflow 
  [Database dump](https://data.stackexchange.com/stackoverflow/query/936241/stack-overflow-data-dump).
> * Click on **fork query**
> * Enter the following query in the editor
```
SELECT * FROM posts WHERE Tags = '<java>'
```
> * Click on **Run query**
> * Download the CSV file by clicking on **Download CSV**

**2. Data Preprocessing**
> * Install MySQL Workbench and follow the steps given in the following link to have a clean and accurate install.
  [MySQL Workbench installation](https://dev.mysql.com/doc/workbench/en/wb-installing.html)
  > * Replace the configuration file **my.ini** in the location **"C:\ProgramData\MySQL\MySQL Server 8.0"** with the new updated file which can be downloaded from github. [Updated file](https://github.com/vinaykashyap1996/MSR/blob/master/Requirements/my.ini)
  > * Restart mysql service in services application in windows.
  > * Open the application MySQL Workbench.
  > * Create a new connection by giving a name and testing the connection in the setup connection page.
  > * Follow these required steps for the further steps to run error-free. These steps are carried as a workaround in windows OS for the existing bug in Workbench regarding import of a file from local system [Import bug](https://bugs.mysql.com/bug.php?id=91891)
  workarounds for other operating systems are also mentioned in the thread.
  ```
  - Copy the stackoverflow data dump csv file earlier downloaded from Stack Exchange to the following location.
  C:\ProgramData\MySQL\MySQL Server 8.0\Uploads
  - In the homepage of MySQL Workbench, click on Edit and  select Preferences.
  - Click on SQL Editor
  - Change the value of "DBMS connection read timeout interval(in seconds):" to "6000"
  - Click on OK.
  - Double click on your connection 
  ```
  > * Right Click in the SCHEMAS window and select create schema
.
> * Give a name for the schema and proceed further.
> * Right click on Tables in your schema and select Create Table.
> * Give a Table Name and write the following query to create a table as needed for our processing.
```
CREATE TABLE `stackoverflow`.`preprocesseddataset` (
id int,
postTypeId int,
exceptedAnswerId int,
parentId int,
creationDate date,
deletionDate date,
score int,
viewcount int,
body text,
ownerUserId int,
ownerDisplayName text,
lastEditorUserId int,
lastEditorDisplayName varchar(200),
lastEditDate date,
lastActivityDate date,
title text,
tags text,
answerCount int,
commentCount int,
favCount int,
closedDate date,
communityOwnedDate date,
contentLiencese text
);
```
```
Note 
- The above steps serve as the necessary prerequisite for MySQL Workbench and further queries.
- Change the schemaname.tablename as and how you have used when creating the same.
```
> * Open and run the SQL file. [Data Preprocessing](https://github.com/vinaykashyap1996/MSR/blob/master/Process/Data_Preprocessing.sql)
```
Note : The preprocessed dataset will now be present in the location as in the query.
```
**Alternative:** The following link contains a python file which performs preprocessing. This is an easier way developed by us to automate the process unlike the method in the paper being reproduced.
[Github link](https://github.com/vinaykashyap1996/MSR/blob/master/Process/Preprocessing.ipynb)

**3. Manual Analysis**
> * Select the Java codes available in the **dataset.csv** file obtained from the previous process.
> * Copy these Java codes and paste in the Java IDEs such as Eclipse or NetBeans for manual analysis.
> * Compile and execute the codes one by one.
> * Categorize the reproducibility status into 6 types
```
1. Reproducible without modification
2. Reproducible with minor modifications
3. Reproducible with major modifications
4. Irreproducible
5. Inaccurate claim
6. Ill-defined issues
```
Note - The following link contains an example of the manual analysis. All the Stack overflow questions in the preprocessed dataset needs to be analysed similarly. [Example code](https://github.com/vinaykashyap1996/MSR/blob/master/Process/Example_codes.pdf)


**4. Validation:**
> * Careful observation should be made on whether the error message mentioned in the Stack Overflow questions is the same as obtained by us in our reproduction effort. 
> * The reproducibility status column in the output should be based on what modification has been done during manual analysis.

# Data
1. Input data:
> * Data dump - This is the Stack Overflow data dump of Java questions extracted from Stack Exchange API. [Data dump](https://github.com/vinaykashyap1996/MSR/blob/master/Data/Input/QueryResults.csv)
> * Preprocessed dataset - This serves as the input for our analysis. [Preprocessed dataset](https://github.com/vinaykashyap1996/MSR/blob/master/Data/Input/preprocessed_dataset.csv)
2. Output data 
> * Reproducibility status - This file contains reported issues which are classified as Reproducible without modification, Reproducible with minor modifications, Reproducible with major modifications, Irreproducible, Inaccurate claim and Ill-defined issues after manual analysis. [Output dataset](https://github.com/vinaykashyap1996/MSR/blob/master/Data/Output/MSR_Result.csv)
> * Visualization - These self explainatory graphs represent the results in a pictorial form. [Result graphs](https://github.com/vinaykashyap1996/MSR/blob/master/Data/Output/Visualization.pdf)

Doc file can be found in this link. [doc file](https://github.com/vinaykashyap1996/MSR/tree/master/Data/Doc)


Note - The graphs are constructed with the help of python, pandas, numpy and matplotlib. [Link to the python script](https://github.com/vinaykashyap1996/MSR/blob/master/Process/Msr-analysis.ipynb)

# Delta
1. Process delta :

The reproducibility of the paper can be summarized as follows.
> * We extracted the Stack Overflow data dump using the link given in the paper. [Link] The link had the query but we had to modify a little to get the required dump.
> * We were able to reproduce most of the steps for preprocessing such as selecting Java questions which had at least one line of code, which had keywords like error, warning, exception, issue, fix, problem, fail and wrong as mentioned in the paper.
> * The process of random sampling was unclear in the paper which made us select the first 400 questions from the preprocessed dataset for manual analysis. 
> * There were no SQL queries or links mentioned in the paper which would transform data dump into preprocessed dataset. We had to write queries on our own.

2. Data delta : 
> * The Stack Overflow dump we used is same as the one used in the paper.
> * In the paper, authors have analysed 400 Java questions manually. In the given timeframe we were able to execute 140 questions manually.
> * Data visualization and the conclusions derived are based on 140 questions we have manually analysed.


# Experiment
# Threats
1. Threats to internal validity relate to the pre-processing followed in the paper. The process of random sampling in the paper involves reducing the number of questions from 1000 questions 400 questions manually. It involves looking at the questions and explicit issues in a manual manner to find duplicates. This reduces the quality of the target sample, and also it is a time consuming process as looking for same strings manually throught the dataset requires a ot of time.

2. The classification approach could be biased and contain some experimental errors. The reproducibility status of the reported issue derived from the classification approach can be different due to manual conflicts. Resolution of conflicts in limited time is very much necessary for the progress of the analysis process. 

# Traces
1. "We identify a total number of 30,528 questions that might have an issue. We randomly select 1,000 questions from them for manual analysis. During manual analysis, we found a significant number of duplicate questions." (Under Dataset preparation section from the paper)

2. "Some of the issues are less likely to reproduce even after several minor and major code level modifications. As mentioned above, two of the authors took part in the manual analysis. When one fails to reproduce the issue, the same question is analyzed by the other author. If both fail, we then conclude the issue as irreproducible In some cases, we spent even a few hours for a single question."(From the paper)

# Theory
1. There is no proper sampling technique or procedure followed to reduce the dataset to the required size. It is evident from the paper that a manual lookout to select questions in the dataset was made in order to eliminate the duplicates. This manual effort is a threat as it results in reduced quality of questions in the dataset and also takes up a lot of time and effort.
This threat can be addressed by providing a method to select the sample automatically without the need to look at all the questions manually in order to eliminate duplicates. By introducing certain new keywords, considering scores given in the StackOverflow website as a condition and several other parameters that can be included as a part of data pre-processing, a better approach and insight can be achieved.

2. The way in which the reproducibility status is decided is based on the decision of authors after manual analysis. The threat is that there might be conflicts among authors regarding the status, which is a threat because it directly affects the results and the insights derived from the graphs. There is also evidence in the paper (also mentioned in the Trace section above) regarding long hours spent on classifying questions.
This threat has been addressed with the help of cross-validation, where the status is decided based on the analysis results of all the authors for each question, but there is no method mentioned which explains how or why there are different status decided by authors for the same question. We address this by designing a method to look into questions with varied decisions regarding their status and resolve such questions individually so that the experimental errors and bias can be limited.

# Feasibility
The focus, importance, and purpose of the experiment being designed will be on the process and not the number of questions which will be manually analyzed. Instead of increasing the number of questions manually analyzed, we will keep the same number of questions that have been analyzed previously, but we will modify the way in which the pre-processing and classification were performed.
We consider only java questions and not any other programming language questions in order to address the threats via our research experiment. The authors believe that the insights can be generalized for other programming language questions in StackOverflow, but we don't address any part of it because the threats chose by us and the way in which they will be addressed do not require us to do so in order for the experiment to fit the course.

# Implementation
1. The preprocessing involves manually reducing the number of questions in order to remove duplicates. A question is defined as a duplicate if the title or body of the issue reported is same. The next step involves removing the questions which have negative scores. The score of a question indicates the difference between total number of upvotes and downvotes given to that question.
> * The below section represents the part of preprocessing involving random sampling by eliminating duplicates and to remove questions with negative scores.
```
SELECT * FROM new_table 
WHERE score>=0 
GROUP BY title,body
LIMIT 400
INTO OUTFILE 'D:/preprocessed_dataset.csv'
FIELDS ENCLOSED BY '"'
TERMINATED BY ','
ESCAPED BY '"' 
LINES TERMINATED BY '\r\n';
```

The removal of duplicates without manual effort makes preprocessing faster. A higher score indicates that the issue being expressed is more clear and well understood by the users. Eliminating questions with negative scores results in removing incomplete, confusing, unclear or vaguely reported issues. This would increase the quality of preprocessed dataset.

2. The reproducibility status is determined from the decisions made by the authors during manual analysis. The mismatch in decisions during the classification of reproducibility status leads to conflicts. For example, one author might classify a questions as irreproducible and another author might classify the same question as reproducible which results in a conflict. 
We propose a set of guidelines to be followed to resolve such conflicts. 
> * The below flowchart is an abstract representation of the conflict resolution process proposed by us.


Let there be a question where both authors have to manually analyse. A conflict occurs when the reproducibilty status classified by both authors are different.
We have set a list of guidelines to resolve the conflicts. Consider a scenario where author 1 classified the status as reproducible and the author 2 classified the same issue as irreproducible. Then author 2 has to follow the below guidelines.
1. When the author 1 has classified the issue as reproducible, he has to provide the steps followed in order to reproduce the issue. 
2. Author 2 has to clearly go through all of them. This will help in preventing and correcting experimental errors made by author 1. 
3. The author 2 has to check if the question has an accepted answer , since it is more likely that the issue can be reproduced.
4. Answer count indicates that the number of users were able to understand or reproduce the question. So, author 2 has to check if the answer count is high or low.
5. In case the conflict still exists the issue should be considered as reproducible. 

Following the guidelines would make the decision making process faster. The conflicts can be resolved without verbal debate or discussions. Reasoning provided by an author for an irreproducible issue helps other authors clearly understand the steps and methods tried by the author. This would reduce the confusions and enhance cross validation.

# Results
1. The elimination of duplicates is performed in a faster way as there is no manual effort. The questions in the preprocessed dataset have a score greater than or equal to zero indicating better quality of questions selected for analysis.
```
CSV to be added
```
2. There is a significant reduction in time taken to resolve a conflict by overturning a decision made by an author. The reasoning included during the classification of a question as irreproducible makes it more clear and understandable.


# Requirements
Same as in baseline

# Process
After following the process as in the baseline, follow the below steps.

> * Right click on Tables in your schema and select Create Table.
> * Give the table name as new_table. Write the following query to create a table as needed for our processing.
```
CREATE TABLE `stackoverflow`.`new_table` (
id int,
title text,
body text,
creationDate date,
score int,
answerCount int
);
```
```
Note 
- Change the schemaname.tablename as and how you have used when creating the same.
```
> * Open and run the updated SQL file with modified queries. [Data Preprocessing]()
```
Note : The preprocessed dataset will now be present in the location as in the query.
```

# Data
1. Input data:
> * Preprocessed dataset - The dataset obtained by using the new SQL file. [Preprocessed dataset]()

Updated Doc file can be found in this link. [doc file]()








