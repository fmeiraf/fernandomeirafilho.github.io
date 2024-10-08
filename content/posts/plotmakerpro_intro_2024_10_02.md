---
title: "Beyond the Hype: 5 Real-World Lessons in Crafting LLM-Powered Products"
date: 2024-10-07T17:55:28+08:00
description: "What I wish I knew if I were starting out with LLMs apps today."
tags: ["LLM", "learnings", "AI"]
type: post
weight: 25
showTableOfContents: true
featured: true
---

## The art of using LLMs

A lot has already been said about where and for what LLMs are good for (an honest summary I like is [here](https://x.com/atroyn/status/1819395539156557892)) but I feel that, even though there new papers and techniques coming out every day on AI space, I haven't heard much about the art (yes, art) of making reliable products with them.

When I decided in early 2024 to start working on [PlotMaker PRO](https://plotmakerpro.com/) I started with few prompts and hit every problem that I could imagine. From suboptimal outputs to latency and frequent hallucinations (i.e., instances where the model generates incorrect or nonsensical information) that are hard to handle on the UI side. My current belief is that AI apps should still be built for scenarios where you assume things will fail and then iteratively improve. There are definitely some practices that can improve things, but also some consequences that will be part of the package.

Below are the things I learned by trying multiple times and reading a lot different papers. I might be missing the new (or maybe not so new) best practices, but this is what I could gather so far. Hope it helps!

### The base: structured outputs

There are many reasons why you would do that, so let me list some of the most important ones and provide you with a simple example:

```python
from pydantic import BaseModel, Field
import instructor
from openai import OpenAI
from typing import List

class Reason(BaseModel):
    reason_name: str
    reason_description: str = Field(description="An overall statement of why this reason is important")
    importance_score: int = Field(description="The level of importance of the reason for using structured outputs")

class ReasonsToUseStructuredOutputs(BaseModel):
    reasons: List[Reason]


# Function to extract user details
def extract_user() -> ReasonsToUseStructuredOutputs(BaseModel):
:
    user = client.chat.completions.create(
        model="gpt-4-turbo-preview",
        response_model=ReasonsToUseStructuredOutputs,
        messages=[
            {"role": "user",
            "content": "List me all the reasons you can think of why structured outputs are important."},
        ]
    )
    return user
```

_Example inspired on [instructor's documentation](https://python.useinstructor.com/why/#pydantic-over-raw-schema)._

- Using structured outputs makes it easier for you to think about the problem you are trying to solve. It also helps you avoid having to tweak the prompt repeatedly to obtain data in the format you need.
- It gives you **extra** room to stir the model into producing the outputs you want by using attributes names and field descriptions that help the model to understand the context.
- It gives you the chance to debug the LLM thinking. Add a couple attributes to understand its train of thought (believe me this is super insightful to improve your prompts further).
- **And for me the most important one:** You will want to use chained/connected prompts to achieve better and more complex results. Structured outputs provide the predictability you need for that. This is actually a good segue to my next learning.

### 1 prompt is not enough

When I first started I had the feeling that if my results were not achieved with a single prompt it would mean that I was doing something wrong: this is so not true. From seeing other products to also self-experimenting, I noticed that using multiple prompts is the norm rather than the exception. When you want to make sure the model follows some guidelines on how to perform the task there a couple things that can go wrong:

- Your system and user prompts can (and I bet they will) get super convoluted, really really fast.
- You might "overfit" your prompts at the point where a bunch of instructions will be ignored by the model, making outputs extremely unpredictable.

One of the things that helped me get through this limitation was letting the model think of the task in steps, feeding on previous outputs. You might have heard that letting the model think of the task in multiple steps is beneficial for results ,also known as chain of thought. I particularly love the [Alpha Codium paper](https://arxiv.org/pdf/2401.08500) implementation. What I found was that isolating important parts of the process as steps let the prompts (system and user) to be way more focused and specialized. This as consequence produced way better results that compouded at each step, at the same time making debug and improvements easier.

Latency is defintely a potential problem here, but if you can balance things and/or use async calls to mitigate it, in my opinion it's a small price to pay for very decent results.

### 1 final output is not enough

Same as above, but think about it: LLMs are probabilistic text generation machines. Given that, if you want your users to get good results - on average - you will need multiple attempts to average them out. This might vary from use case to use case, but having this in mind will help you to design systems that will take advantage of having many "thinking heads (or agents)" getting into the best consensus for you.

Agents = system prompt + user prompts + model responses. Mostly the system prompt and the first user prompt will define how an "agent" should behave.

### Cheaper and smaller models are only good for testing

You will always want to use the best model available. It's tempting to delegate some parts of the process to cheaper/smaller models, but in the end, you will always get better results using the larger/better-performing ones. Larger models generally have a broader knowledge base and better capabilities, which leads to more accurate and reliable outputs. When you use multiple steps like mentioned above, one poor response is enough to lead the process to a suboptimal output. Also, cheaper models provide worse results and often incorrectly formatted responses.

**Important note:** in the overall scheme of things when testing prompts, these models are actually a great way to have cheap attemps and test how the **system works with your prompts as a whole**, but I would always use the best model available to finish the process and draw the final conclusions.

### Think like a human

The thing I like the most about using multiples prompts (as multiples steps) is that it enables you to add a lot of details and personality to each part of the process. This resembles how we normally do things, making it easier to test workflows that are already known. By using multiple 'agents,' you can achieve faster results.

## Final thoughts

I've been playing with LLMs for the past 8 months now, and I still feel like I'm learning something new every day. I hope you enjoyed it and that it helps you to be more confident on your next steps on building AI apps!

Couple things before you leave:

If you (or anyone you know) is looking for a good and fast way to generate plots, you can check [PlotMaker PRO](https://plotmakerpro.com/). It's still in beta, but it's already live and working. It allows you to generate plots with a couple clicks.

I plan to keep sharing more learnings here so if you want to be notified of new posts, consider subscribing to the newsletter!

See you on the next one :)
