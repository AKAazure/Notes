## Dependency Parsing

- The syntactic structure is described solely in terms of directed binary grammatical relations between the words
- Relations among the words are illustrated with **directed**, **labeled typed arcs(edges)** from heads to dependents
- A ***root*** node explicitly marks **the root of the tree**, the head of the entire structure
- The internal structure of the dependency parse consists solely of directed relations between lexical items in the sentence
- The Universal Dependencies (UD) project
  - The UD project provides dependency relations that are linguistically motivated
- Dependency Formalisms
  - A dependency structure is a directed graph 𝐺 = (𝑉, 𝐴)
    - A set of vertices 𝑉. exactly to the set of words in a given sentence
    - A set of arcs 𝐴. Arcs are ordered pairs of vertices
      -  head-dependent and grammatical function relationships between the elements in $V$
   - A dependency tree is a directed graph that satisfies the following constraints
     - There is a **single designated** root node that has no incoming arcs
     - each vertex has exactly one incoming arc
     - a unique path from the root node to each vertex in 𝑉.
   - A dependency tree, by dentition 
     - Each word has a single head,
     - The dependency structure is connected,
     - There is a single root node from which one can follow a unique directed path to each of the words in the sentence.

### Dependency treebanks
- Dependency treebanks have been created manually or with help of parsers 
- can also be translated from constituent-based treebank
  - The head is the word in the phrase that is grammatically the most important.
    - Mark the head child of each node in a phrase structure
    - In the dependency structure, make the head of each non-head child depend on the head
    - Grammatical relations and function tags in the constituent-based parse trees can be used to label the edges in the resulting dependency tree

### Transition-Based Dependency Parsing
- This architecture is based on **shift-reduce parsing**, a paradigm originally  developed for analyzing programming languages
  - Oracle is a classifier to make one of the three decisions: LeftArc, RighArc, Shift
- Shift-reduce parsing
  - The parser walks through the sentence left-to-right, successively shifting items from the buffer onto the stack
  - LeftArc and RightArc are REDUCE actions