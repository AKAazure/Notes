# Quantitative Day Trading from Natural Language using Reinforcement Learning

## Abstract
- stock price movements are **highly stochastic**
- propose a **deep reinforcement learning approach** that makes **time-aware** decisions to trade stocks while optimizing profit using textual data

## Background

### feinforcement Learning and Natural Language Processing 
- information extraction (Qin et al., 2018), 
- social media analysis (Zhou and Wang, 2018), 
- text classification (Wu et al., 2018a), 
- extractive (Narayan et al.,2018) and abstractive (Chen and Bansal, 2018) text summarization, 
- neural machine translation (Wuet al., 2018b), 
- text-based games (He et al., 2016a;Ammanabrolu and Riedl, 2019), 
- knowledge-based question answering (Hua et al., 2020)

### Reinforcement Learning in Finance
- portfolio management (Filos, 2019; Almahdi and Yang, 2019), 
- equity asset reallocation (Meng and Khushi, 2019; Katongo and Bhattacharyya, 2021),
- cryptocurrency trading (Jiang et al., 2017; Jiang and Liang, 2017; Lucarelli and Borrotti, 2019; Ye et al., 2020)

### Drawbacks
- depend largely on **the quality of external feature representations**
  -  (for instance, sentence embeddings (Ye et al., 2020)) of text. 
- only use prices exhibit lower practical applicability to real-world trading, owing to the lack of information in prices alone

## 3 Problem Description
- $S=\{s_1,s_2,\dots,s_N\}$ 
  - a set of N stocks
- Aim to design 
  - a ***trading agent*** that learns to interact with 
  - ***the stock market environment*** by leveraging 
  - ***stock-affecting signals*** present across **financial news** items and **tweets** to trade stocks.
- As the stock markets and the underlying factors that drive stock prices are **not fully-observable**, **Partially Observable MDP (POMDP)** provides a natural generalization of the MDP to model the stock trading environment 
- 