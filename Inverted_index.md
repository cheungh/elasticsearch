### Apache Lucene Inverted Index
Why Apache Lucene Inverted Index is very efficient and fast search such as term-based queries?
Its approach is different than RDBMS.
It cares about the terms in the index to the documents.
NOT as RDBMS key to the document


#### 1. Document number and position
<pre>
Document 1: Howard loves program  
Document 2: Jake Program on weekend  
Document 3: Sally love food 

The term "Program" is 1_3 and 2_2  
document presents in => 1 and position => 3   
document presents in => 2 and position => 2  
</pre>

#### 2. Counts of term
Program => 2 times (the term program appears 2 times in all 3 documents

### In the RDBMS case
Key 1 => Document 1
Key 2 => Document 2
Key 3 => Document 3

## Aggregate query 

RDBMS have to gather all the documents before it can aggregate the result.
Select * from Documents WHERE document contains the word "Program"
VS
Inverted Index already indexed the answer : 2

Imagine we are querying against a table with 1 billion rows.

Which approach is faster? The answer is very apparent. The inverted index.
