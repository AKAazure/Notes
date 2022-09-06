# Constituency Grammars and Parsing

- syntax refers to the way words are arranged together
- **Context-free grammars** (CFG) are the backbone of many formal models of the  syntax of natural language (and computer languages).
- Syntactic constituency is the idea that **groups of words** can **behave as single units**, or **constituents**
  - Examples:
    - Noun Phrase (NP)
      - Harry the Horse
      - the Boardway coppers
      - a high-class spot such as Mindy's
    - Prepositional Phrase (PP)
      - The whole prepositional phrase behaves as a single unit, and can be placed at  **different places** in a sentence
      - on Sep 4th
      - *On September seventeenth*, I’d like to fly from Atlanta to Denver
      - I’d like to fly *on September seventeenth* from Atlanta to Denver
      - I’d like to fly from Atlanta to Denver *on September seventeenth*

### Context-Free Grammar (CFG)
- CFG is also called Phrase-Structure Grammars, and the formalism is equivalent to **Backus-Naur Form (BNF)** (是的，就是那个BNF)
- A context-free grammar consists of
  - A set of rules or productions,
    - Each rule has an arroe (→)
      - a single non-terminal → an ordered list of one or more terminals and non-terminals.
      - **NP** -> **Det** **Nominal**
      - **Noun** -> *flight*
  - a lexicon of words and symbols
- The symbols in a CFG are divided into two classes
  -  Terminal: The symbols that correspond to words in the language
     -  Syntax树的叶节点
  -  Non-terminals: The symbols that express abstractions over these terminalsu
