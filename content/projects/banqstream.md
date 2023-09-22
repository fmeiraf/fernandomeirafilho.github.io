---
title: BanQstream
type: page
---

### Live at

💸 [**banqstream.com**](banqstream.com)

### Background

This was a project I created to exercise some of the new trends on LLMs. I was particularly interested in LLM's capacity to extract information from raw inputs and further transform it either by grouping similar things ( embeddings > ANN > etc) to transforming and proposing new meaning to data (assigning automatic labels for example).

## Subjects to explore

- Basic LLM functionalities
- Embedding + ANN + clustering
- LLM finetuning

### Quick overview

This is an attempt to create a finance/budgeting assistant. This will be using LLMs to parse raw text coming from account statements and feeding this into a database used for further analysis. Among the features on this app:

- Parse raw account transactions
- Assign categories based on transactions names (try to make this as consistent and flexible as possible)
- Split big inputs from customers that might exceed model max tokens limitations (i.e 4096 etc)
- Simple dashboard for data visualization
- Enable user to talk to its own data through chat UI

### To-Do

- Design a dedicated UI, possibly with fastAPI and React.
- Make data parsing faster (maybe experiment with different models?)
- Experiment with fine-tuned models for the AI assistant.
- Enhance the UI with added functionalities for editing transactions.
