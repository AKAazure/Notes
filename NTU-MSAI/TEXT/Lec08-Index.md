## Index Construction and Compression
- When the data collection is large
- Single-pass in-memory indexing (SPIMI)
  - Generate separate **dictionaries** for each block of memory
  - Accumulate postings in postings lists as they occur 
- Index merge
  - separated indexes (one for each block) can then be merged into one big index for the document collection.
- Distributed Indexing: If the document collection is huge
  - Partition by documents (local index organization)
    - Documents are distributed to different subsets
    - One index is constructed for each subset of documents
    - More widely adopted in search engines
  - Partition by terms (global index organization) 
    - The dictionary of index terms are partitioned into subsets
    -  One machine handles a subrange of terms; 
  
#### Term-partitioned distributed indexing in parallel
 -  Break the input document collection into splits
 -  Maintain a master machine directing the indexing job
 -  we use two sets of parallel tasks
    -  ***Parsers***
       -  Reads a document at a time, and emits <term, docID> pairs
       -  Parser writes pairs into $j$ ***partitions***, each partition is for a range of terms’ first letters 
          -  (e.g., a-f, g-p, q-z) – here j = 3.
    -  ***Inverters***
       -  Collects all <term, docID> pairs for one term-partition (e.g., a-f) 
       -  Sorts and writes to postings lists
    - Master assigns a split of documents to an idle parser machine
 - ***MapReduce***
   - Map Phase - Segment Files  - Reduce Phase - Postings
   - Schema of map and reduce functions
     - Map phase: $input$ → $list(𝒌, 𝒗)$
     - Reduce phase: $(𝒌, list(𝒗))$ → $output$

#### Dynamic indexing
- Document collections may not be static
  - Document may add, delet, modify
- This means that the dictionary and postings lists are to be modified:
- Document updates: delete and reinsert

### Index Compression

#### Lossless vs. lossy compressions
- Loseless compression
- Lossy compression
  - Discard some information
  - Prune postings entries that are unlikely to turn up in the top 𝑘 list for any query.
- Vocabulary vs. collection size
  - How big is the term vocabulary?\In practice, the vocabulary will keep growing with the collection size
- Heaps’ law: $M=kT^b$
  - $M$ is the size of the vocabulary, 
  - $T$ is the number of tokens in the collection
  - In a log-log plot of vocabulary size 𝑀 vs. 𝑇, Heaps’ law predicts a line with slope about $\frac{1}{2}$
- Zipf’s law
  - The $𝑖$-th most frequent term has frequency proportional to $\frac{1}{i}$
  - $cf_o ∝ \frac{1}{i} =\frac{K}{i}$, where 𝐾 is a normalizing constant
    -  Linear relationship between $\log{𝑐𝑓_𝑖}$ and $\log{𝑖}$
  - $cf_i$ is collection frequency: the number of occurrences of the term $t_i$ in the collection.
- Index Compression
  - Compressing the term list: Dictionary-as-a-String
    - Store dictionary as a (long) string of characters
      - Pointer to next word shows end of current word
    - 4 bytes per term for document frequency
    - 4 bytes per term for pointer to Postings
    - 4 bytes per term for pointer to Postings
    - Avg. 8 bytes per term in term string
    - Total size of dictionary: 400K terms x 19 = 7.6 MB
- Can we do better? → Blocking
  - Store pointers to every $𝑘$th term string
- Front coding – more compression
  - Sorted words commonly have long common prefix
    - 8automat*a1e2ic3ion
- Postings compression
  - The postings file is much larger than the dictionary
    - A posting for our purposes is a docID.
    - : two conflicting forces
      - A rare term 
      - A common term, so 20 bits per posting is too expensive
    - We store the list of docs containing a term in increasing order of docID
    - Variable length encoding