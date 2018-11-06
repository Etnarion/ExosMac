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
### D.
Consider the following collection of four documents:
Doc1: Shared Computer Resources
Doc2: Computer Services
Doc3: Digital Shared Components
Doc4: Computer Resources Shared Components
#### D.1 What documents are retrieved, with the Boolean model, with the query “Computer AND NOT Components”?
Doc1, Doc2.
#### D.2 Compute the idf value of the terms “Computer” and “Components” (consider we have only these 4 documents in the collection).
computer idf = Log10(N/df) = Log10(4/3) = 0.1249387366
components idf = Log10(N/df) = Log10(4/2) = 0.30102999566
#### D.3 Compute the vector model representation of Doc4 using tf-idf weights (logarithmic tf* idf).
Computer : (1+Log10(1/4))*Log10(4/3) = 0.04971812192  
Resources : (1+Log10(1/4))*Log10(4/2) = 0.11979187908  
Shared : (1+Log10(1/4))*Log10(4/3) = 0.04971812192  
Components : (1+Log10(1/4))*Log10(4/2) = 0.11979187908  
Score(q,d) = 0.04971812192 + 0.11979187908 + 0.04971812192 + 0.11979187908 = 0.339020002
#### D.4 Compute the vector model representation of the query “Computer Components” using only raw tf and no idf.

