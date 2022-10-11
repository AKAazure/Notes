## Parts of Speech and Named Entities
- Parts of speech (POS) refers to word classes such as Noun, Verb, Adjective, etc.
  - Also known as **lexical categories**, **word classes**, **morphological classes**, **lexical tags**
- Named entity is proper name for person, location, organization, etc. 
- Sequence labeling 
  - POS tagging: takes a sequence of words and assigns each word a POS like NOUN or VERB
  - Named Entity Recognition (NER): assigns words or phrases tags like PERSON, LOCATION, or ORGANIZATION.
  
#### POS Tagging
- There are multiple POS tagsets defined (what POS tags can be assigned)
  - Open class
    - Nouns and verbs are among open classes 经常会有新词比如iPhone
    - Examples:
      1. Nouns
      2. Verbs
      3. Adj
      4. Adv
      5. Propn Proper Noun 特有名词
      6. Intj Interjection 语气词
  - Closed Class
    - Closed classes are those with **relatively fixed membership** 很少有新词
    - Examples:
      1. Adp adposition 方位 in,on,by
      2. Auxiliary 助词 can,may,should,are
      3. Cconj 连词 and, or, but
      4. Det 冠词 a,an, the
      5. Num 数
      6. Part
      7. Pron 代词
      8. Sconj 从句连词
  - Other
    - Examples:
      1. PUNCT 标点符号
      2. SYM  其他符号
- ***Part-of-speech tagging*** Part-of-speech tagging is the process of assigning a part-of-speech to each word in a text.
- Why POS tagging is challenging
  - Words are ambiguous — have more than one possible part-of-speech 

#### POS Tagging with Hidden Markov Model (HMM)
- Given a sequence of words, it computes a probability distribution over possible sequences of labels and chooses the best label sequence.
- POS Tagging in probabilistic view:
  - Consider all possible sequences of tags (each tag for one word)
  - Out of this universe of sequences, choose the tag sequence which is most probable given the observation sequence of 𝑛 words $w_1 \dots w_n$
- Markov Chains
  - a set of $N$ states
    - $Q = q_1q_2\dots q_N$
  - Transition probability matrix A
    - $A=a_{11}\dots a_{NN}$
  - initial probability distribution
    - $\pi = \pi_1 \pi_2 \dots \pi_N$
- Hidden Markov Model
  - A Markov chain is useful when we need to compute a probability for a sequence of observable events
  - In many cases, however, the events we are interested in are hidden
    - $Q = q_1q_2\dots q_N$
    - $A=a_{11}\dots a_{NN}$
      - P(Noun|Adj) P(Verb|Adj)
    - $O = o_1o_2\dots o_T$
      - a sequence of $T$ observations, each one frawn from a vocabulary $V=v_1,v_2,\dots,v_V$
    - $B = b_i(o_t)$
      - a sequence of observation liklihoods, also called emission probabilites,
        - expressing the probability of $o_T$ being generated from a state $q_i$
        - P(table | Nonun)
    - $\pi = \pi_1,\pi_2,\dots,\pi_N$
- Markov Assumption: the probability of a particular state depends only on the previous state 
  - $P(q_i=a|q_1 \dots q_{i-1}) = P(q_i=a|q_{i-1})$
- Output Independence Assumption: the probability of an output observation $𝑜_𝑖$ depends only on the state $𝑞_𝑖$ that produced the observation and not on any other states or any other observations
  - $P(o_i|q_{1:T};o_{1:T}) = P(o_i|q_i)$
- Decoding: For any model, such as an HMM, that contains hidden variables, the task of **determining the hidden variables sequence** corresponding to the sequence of observations is called **decoding**.
- Out of all possible sequences of $n$ tags $t_1\dots t_n$ the single tag sequence such that $P(t_1 \dots t_n | w_1 \dots w_n)$ is highest.
  - $\hat t_{1:n} = \argmax_{t_{1:n}}{P(t_{1:n} | w_{1:n})}$
- Use **Bayes rule** to transform this equation into a set of other probabilities that are easier to compute
  - $\hat t_{1:n} = \argmax_{t_{1:n}}{P(t_{1:n} | w_{1:n})} = \argmax_{t_{1:n}}{\frac{P(w_{1:n}|t_{1:n})P(t_{1:n})}{P(w_{1:n})}} \\≈\argmax_{t_{1:n}}{P(w_{1:n}|t_{1:n})P(t_{1:n})}≈\Pi^{n}_{i=1}{P(w_i|t_i)P(t_i|t_{i-1})}$
- Computing the two kinds of probabilities
  - Tag transition probabilities
    - $p(t_i|t_{i-1}) = Count(t_{i-1},t_{i})/Count(t_{i-1})$
  - Word likelihood probabilities
    - $p(w_i|t_i) = Count(t_i,w_i)/Count(t_i)$
  
#### The Viterbi algorithm
1.  first sets up a probability matrix or lattice
    1.  One column for each observation $𝑜_𝑡$ (a word)
    2.  One row for each state $𝑞_𝑡$ (tag) in the state graph
    3.  Each cell of the lattice, 𝒗𝒕(𝒋), represents the probability that the HMM is in state 𝑗 after seeing the first 𝒕 observations and passing through the most probable state sequence 𝒒𝟏, … , 𝒒𝒕−𝟏, given the HMM model parameters
```
dp = list(t,v)
# init
for i in range(t):
    dp[t][0] = P_start * b_s(o_1)
    backpointers[i,0] = 0
for word in range(1,v):
    for tag in range(t):
        dp[tag,word] = max(dp[tag,word-1]*a(tag,t)*b_s(o_word) for i in range(t))
        backpointer ...
bestprob = max(dp[:,-1])

```

### NER Named Entity Recognition
- A named entity is roughly anything that can be referred to with a proper name
- The task of named entity recognition (NER) is to find **spans of text** that constitute proper names and tag the type of the entity. 
  - common tags are `PER` `LOC` `ORG` `GPE`
- POS Tagging vs NER
  - Differences between part-of-speech tagging and NER
    - In POS tagging, each word gets **one tag**
    - In NER, we **do not know the boundary** of names, before we can label them
  - Similarities between POS tagging and NER
    - The same word may have different POS/NE tags, Washington可以是人名、DC、州
    - Both POS tagging and NER require surrounding words as **context** to make the tagging
    - Both POS tagging and NER work at sentence level 
- Sequence labeling for NER
  - BIO tagging
    - B: begin I:inside O:outside
  - Variants: IO BIOES(E:ending, S: single), BIOEU(U:unit)