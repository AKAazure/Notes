
## Boolean Retrieval

- ***Information Retrieval***
  - Information Retrieval (IR) is finding material (usually documents) of an  **unstructured nature** that satisfies an **information need** from within **large collections**.
    - **Information need**: The topic about which the user desires to know more
    - **Query**: What the user conveys to the computer in an **attempt to communicate** the information need
    - **Relevant document**: user perceives as **containing information** of value with respect to the information need
- ***Boolean Retrieval***
  - the simplest model to base an information retrieval system on queries are Boolean expressions
    - Brutus **AND** Caesar
    - The search engine return all documents that satisfy the Boolean expression
    - Example: Email search, legal search
- Why not use "grep"?
  - Slow (for large corpora)
  - grep is line oriented, IR is document-oriented
  - cannot perform **"NOT"**
  - cannot perform compound operations (find the word "Romans" near "countryman")
- ***Inverted index***
  - For each term $t$, we store a list of all documents that contain $t$
    - Each document is identified by a unique docID
  - 流程
    1. Tokenizer -> Token stream `[Friends] [Romans] [countryman]`
    2. Linguistic modules -> Modified tokens `[friend] [roman] [countryman]`
    3. Indexer -> Inverted index  
        ```
        [friend] -> doc: 2,4,...; 
        [roman] -> doc: 1,2,...; 
        [countryman] doc: 13,26, ...
        ```
  - Where do we pay in storage?
    - A simple inverted index contains
      - List of **terms** and their **document frequency**
        - Document frequency is how many documents contain this term
      - The postings lists (docIDs)
    - Later talks about compressing
  - **Query processing: AND**
```python3
def intersect(pt1,pt2):
    answer = []
    while pt1!=None and pt2!=None:
        if pt1.docID == pt2.docID:
            answer.append(pt1.docID)
            pt1=pt1.next
            pt2=pt2.next
        elif pt1.docID<pt2.docID:
            pt1 = pt1.next
        else:
            pt2 = pt2.next
    return answer
```
  - Boolean queries: Exact match
    - The **Boolean retrieval model** answers query in a Boolean expression:
      - Boolean Queries are queries using AND, OR and NOT to join query terms
        - documents are viewed as a **set** of words (不考虑词汇顺序、频率等)
        - is **precise**
  - ***Query optimization***
    - What is the best order for query processing?
    - Process in order of **increasing frequency**:
      - Start with smallest set, then keep cutting further.

### Phrase queries
- Many more queries are implicit phrase queries
- A first (not so good) attempt: ***Biword indexes***
  - Index **every consecutive pair** of terms in the text as a phrase
  - Each of these biwords is now a **dictionary term**
  - **Issues for biword indexes**
    1. False positives
         - match all biwords instead of a phrase
    2. Index blowup due to bigger dictionary
   - Biword indexes are not the standard solution, but can be **part of a compound strategy**
- **Positional indexes**: a better solution 
  - In the postings, store, for each term the position(s) in which tokens of it appear:
  - For phrase queries, we use a merge algorithm recursively at the document level
  - **Positional index size**
    - Positional index is **large** even after position values/offsets compression.
      - 2~4 times as large as a non-positional index
    - Nevertheless, a positional index is **now standardly used** because of the  power and usefulness of phrase and proximity queries
    - Biword indexing can be part of a **compound strategy**

### Tolerant Retrieval
- “Tolerant” retrieval
    - What if a query word is designed to match multiple words?
    - What if a query word is wrongly spelled?
- Dictionary data structures for inverted indexes
  - The dictionary data structure stores 
    - Term vocabulary, 
    - Document frequency for each term 
    - Pointers to each postings list
```
| Brutus    | 8 | -> 1,2,4,11,31,...|
| Caesar    | 8 | -> 1,2,4,5,6, ... |
| Calpurnia | 8 | -> 2,31,54,101    |

|<- Dictionary->|<-   Postings    ->|
```
  - Dictionary data structures 2 options
    - Hash Table
      - Pros:
        - **Lookup** is faster than tree: O(1)
      - Cons:
        - No easy way to find **minor variants**
        - No **prefix search**
        - If vocabulary keeps growing, need to occasionally do the expensive operation of **rehashing everything**
    - Tree
      - Pros:
        - Solves the prefix problem (all the terms starting with ‘hyper’)
      - Cons:
        - Slower than hash 
        - Rebalancing binary trees is expensive
- “Tolerant” retrieval
  - Wild-card queries
    - mon* : 所有mon开头的词
  - Spelling correction
    - context-sensitive
  - Soundex
    - 基于sound的拼写

#### Wild-card queries: *
- mon*:
  - Easy with binary tree (or B-tree) lexicon
  - Retrieve all words in range: mon ≤ w < moo
- *mon:
  - Maintain an additional B-tree for terms backwards.
  - Can retrieve all words in range: nom ≤ w < non
- how about pro*cent?
  - Get pro*
  - Get *cent
  - Merge two
- **Permuterm** Index
  - for a dictionary term hello, its permuterms are 
        - hello$, ello$h, llo$he, lo$hel, o$hell
  - Permuterm query processing
    - Enumerate all **k-grams** (sequence of k chars) occurring in any dictionary  term. 
    - K-gram index is a second inverted index considering each dictionary term as a short document. 
  - Processing wild-cards with k-gram index 
    - Query mon* can be run as `$m AND mo AND on`
    - Processing mon* with k-gram index: 
      1. Gets dictionary terms that match AND version of the wildcard query
      2. Post-filter these terms against the wild-card query, to avoid **false positives**
    - Compared to permuter, K-gram indexing is fast and space efficient
    - Used Techs
      - B-tree
      - Permuterm index
      - K-gram index
    - Wild-cards can result in very **expensive** query execution

#### Spell correction
- Two principal uses in the context of information retrieval
  - Correcting document(s) being indexed
  - Correcting user queries
- Two main cases:
  - Isolated word
  - Context-sensitive
    - Look at surrounding words senetences
- Soundex for information
  - Step 1: Retain the first letter of the word.
  - Step2: Change all occurrences of the following letters to '0' (zero):
    - 'A', E', 'I', 'O', 'U', 'H', 'W', 'Y’.
  - Step 3: Change letters to digits as follows:
    - ▪ B, F, P, V → 1
    - ▪ C, G, J, K, Q, S, X, Z → 2
    - ▪ D,T → 3
    - ▪ L → 4
    - ▪ M, N → 5
    - ▪ R → 6
  - Step4: Remove all pairs of consecutive digits.
  - Step5: Remove all zeros from the resulting string.
  - Step6: Pad the resulting string with trailing zeros and return the first four positions, which will be of the form \<uppercase letter\> \<digit\> \<digit\> \<digit\>.
  