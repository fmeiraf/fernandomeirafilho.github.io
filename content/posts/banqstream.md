---
title: "What I have learned building an LLM based application (so far)"
date: 2023-09-22T17:55:28+08:00
description: "Learning from building an LLM based application (BanqStream)."
tags: ["LLM", "Machine Learning", "NLP", "Infra"]
type: post
weight: 25
showTableOfContents: true
---

## The start and motivations

It's AI season! 🚀

Jumping on this bandwagon, I decided to build [BanQstream](www.banqstream.com). This is an attempt to create a finance/budgeting assistant - something I wanted for myself to get rid (finally) of my spreadsheets. My primary aim with this project is to dive deep into LLMs and see what they've got to offer.

This is what [BanQstream](www.banqstream.com) can do right now:

- Parse the data from any bank account or credit card you own.
  - No connections with bank accounts required > A nod to privacy.
- Standardize names for similar transactions, making your financial overview a breeze.
- Suggest spending categories for your transactions.
- Ask questions about your finances! Fetch answers and visual breakdowns of all your transactions stored on **BanQstream**.

## Lessons learned so far

### Using streamlit

**The good**

I had a very nice time learning and using streamlit for this project. Streamlit is very lightweight and very fast to iterate and interact with ML projects and ideas. One of they key things I liked the most on Streamlit is how easy it's to play with its components and mostly the shared state around multi-page apps. The components also look good and the community has built awesome addons for you to add extra functionality without needing to code new components yourself.

**The not so good**

Here I would not put anything on Streamlit's fault but myself. Streamlit purpose is to be a quick tool to share data apps in pure python and it delivers beutifully. My problem was that I had some design ideas on my head since I thought about BanQstream as a concept and it's hard to deliver a functional web app only with Streamlit - it looks messy even though you have some good resources to organize things.

I will need to move to Javascript with the front end to solve this.

**A tip**

If you are creating ML apps - even for API wrappers - I would totally recommend that you check the cached functions ([st.cache_data](https://docs.streamlit.io/library/api-reference/performance/st.cache_data) and [st.cache_resource](https://docs.streamlit.io/library/api-reference/performance/st.cache_resource)). This will speed up the experience of your users (and prevent you for running a bunch of unecessary calls).

### Using OpenAI

[BanQstream](www.banqstream.com) leverages Large Language Models (using OpenAI's API) to parse information from raw transactions (via pure text). This results in waiting times that only scale up as the number of transactions the user tries to parse using the app increases. I believe this would apply to pretty much any type of parsing tasks using LLMs - =/.

Different from the most common use cases, parsing information into formats that can be stored and used for computation require specific data formats, which means **imcomplete answers don't make sense or are not useful for the final user**. This pretty much rules out a common solution when using API calls which is the use of streaming.

At this point I am leaning towards:

- Test different LLMs that could potentially have inference APIs that are faster.
- Train/Fine-tune my wn model for this specific case and have it hosted in a place where I can control better the server configurations, thus being able to improve inference time (paying the price - of course).

## Next Steps

This project is my playground to experiment with LLM applications across various challenges. Currently, I'm intrigued by:

- Basic LLM functionalities.
- Embedding + ANN + clustering.
- LLM fine-tuning.

And here are the possible next steps for this project (although, a heads up, not all may see the light of day):

- Design a dedicated UI, possibly with fastAPI and React.
- Make data parsing faster.
- Experiment with fine-tuned models for the AI assistant.
