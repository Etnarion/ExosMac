# Exercises MAC
## Part 1
### A. For a conjunctive query (AND), is processing postings lists in order of size guaranteed to be optimal? Explain why it is, or give an example where it isn’t.
It is most often a good thing to do but it is not guaranteed to be optimal. If the query is Alain AND Didier AND Lucien with the following postings list:
```
Alain = 1, 4, 39
Didier = 2, 4, 98, 102
Lucien = 40, 66, 109, 220, 234
```
It would be optimal to process first Alain AND Lucien because we would immediatly see that there is no intersection between Alain and Lucien therefore no match for the query.
### B. How should the Booleanquery x AND NOT y be handled? Why is naive evaluation of this query normally very expensive? Write out a postings merge algorithm that evaluates this query efficiently.
A naive evaluation of this query would be increase the worst case complexity to O(x*y) because it would have to process each listing of y for each listing of x.  
The algorithm bellow show a better way to handle this query.  
```
Intersect(x, y)
    answer <- []
    while x != NIL and y != NIL
    do if docID(x) = docID(y)
        then 
            x <- next(x)
            y <- next(y)
        else if docID(x) < docID(y)
            ADD(answer, docID(x))
            x <- next(x)
        else 
            y <- next(x)
            
```
### C. What is the idf of a term that occurs in every document? Compare this with the use of stop word lists.
idf = log10(N/N) = log10(1) = 0  
A stop word list
### D. Consider the following collection of four documents:  
Doc1: Shared Computer Resources  
Doc2: Computer Services  
Doc3: Digital Shared Components  
Doc4: Computer Resources Shared Components
#### D.1 What documents are retrieved, with the Boolean model, with the query “Computer AND NOT Components”?
Doc1, Doc2.
#### D.2 Compute the idf value of the terms “Computer” and “Components” (consider we have only these 4 documents in the collection).
computer idf = Log10(N/df) = Log10(4/3) = 0.12  
components idf = Log10(N/df) = Log10(4/2) = 0.3  
#### D.3 Compute the vector model representation of Doc4 using tf-idf weights (logarithmic tf* idf).
Computer : (1+Log10(1))*Log10(4/3) = 0.12  
Resources : (1+Log10(1))*Log10(4/2) = 0.3  
Shared : (1+Log10(1))*Log10(4/3) = 0.12  
Components : (1+Log10(1))*Log10(4/2) = 0.3  
Services : 0  
Digital : 0  
Doc4 = (0.1249387366, 0.3, 0.12, 0.3, 0, 0)
#### D.4 Compute the vector model representation of the query “Computer Components” using only raw tf and no idf.
query = (1, 0, 0, 1, 0, 0)
#### D.5 Compute the similarity between the query “Computer Components” and Doc4 with the cosine similarity measure.
With raw tf only :  
CoCo: "Computer Components" query  
Doc4: Doc4 document  
|CoCo| = sqrt(1 + 0 + 0 + 1 + 0 + 0) = sqrt(2)  
|Doc4| = sqrt(0.12² + 0.3² + 0.12² + 0.3² + 0 + 0) = 0.46  
normalized CoCo = (1/sqrt(2), 0, 0, 1/sqrt(2), 0, 0) = (0.7, 0, 0, 0.7, 0, 0)  
normalized Doc4 = (0.12/0.46 + 0.3/0.46 + 0.12/0.46 + 0.3/0.46 + 0 + 0) = (0.27, 0.65, 0.27, 0.65, 0, 0)  
cos(CoCo, Doc4) = 0.7*0.27 + 0 + 0 + 0.7*0.65 + 0 + 0 = 0.65  

### E. 
<pre>
|         |                Query                  |               Document              |  
| word    | tf | wf | df      | idf   | qi=wf-idf | tf | wf  | di=normalized wf | qi*di |  
|---------|----|----|---------|-------|-----------|----|-----|------------------|-------|  
| digital | 1  | 1  | 10'000  | 1'000 | 1'000     | 1  | 1   | 1/1.92           | 520   |  
| video   | 0  | 0  | 100'000 | 100   | 0         | 1  | 1   | 1/1.92           | 520   |  
| cameras | 1  | 1  | 50'000  | 200   | 200       | 2  | 1.3 | 1.3/1.92         | 670   |  
</pre>
  
Final similarity score = 0.7 * 0.52 + 0 + 0.7 * 0.67 ~= 0.83

## Part 2: Evaluation in Information Retrieval
### F. 
System1 NNNNR RRRRN  
System2 NRRNR RNNNN  
### a. What is the Precision, Recall, and F-Measure of each system for the top 10 documents? Comment on your results.  
<pre>
System1    N    N    N    N    R    R    R    R    R    N
Recall    0.0  0.0  0.0  0.0  0.17 0.33 0.5  0.67 0.83 0.83  
Precision 0.0  0.0  0.0  0.0  0.2  0.33 0.42 0.5  0.55 0.5  
F Measure 0.0  0.0  0.0  0.0  0.18 0.33 0.46 0.57 0.66 0.62  
  
System2    N    R    R    N    R    R    N    N    N    N
Recall    0.0  0.17 0.33 0.33 0.5  0.67 0.67 0.67 0.67 0.67  
Precision 0.0  0.5  0.67 0.5  0.6  0.66 0.57 0.5  0.44 0.4  
F Measure 0.0  0.25 0.44 0.39 0.54 0.66 0.61 0.57 0.53 0.5  
</pre>

