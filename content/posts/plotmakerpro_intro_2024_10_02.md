---
title: "Beyond the Hype: 5 Real-World Lessons in Crafting LLM-Powered Products"
date: 2024-10-02T17:55:28+08:00
description: "What I wish I knew if I were starting out with LLMs apps today."
tags: ["LLM", "learnings", "AI"]
type: post
weight: 25
showTableOfContents: true
featured: true
---

## The art of using LLMs

A lot has already been said about where and for what LLMs are good for (actually a honest summary I like is [here](https://x.com/atroyn/status/1819395539156557892)) but I feel that, even though there new papers and techniques coming out every day on AI space, I haven't heard much about the art (yes, art) of making reliable products with them.

When I decided in early 2024 to start working on [PlotMaker PRO](https://plotmakerpro.com/) I started with few prompts and hit every problem that I could imagine. From suboptimal outputs, to latency and frequent hallucinations that are hard to handle on the UI side. My current belief is that AI apps should still be built on scenarios where you need assume things will fail and then iteratively be improved. There are definetely some practices that can improve things, but also some consequences that will be part of the package.

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

- Using structured outputs makes it easier for you to think about the problem you are trying to solve at the same time avoinding you to rumble around with the prompt to obtain data in the format you need.
- It gives you **extra** room to stir the model into producing the outputs you want by using attributes names and field descriptions that help the model to understand the context.
- It gives you the chance to debug the LLM thinking. Add a couple attributes to understand its train of thought (believe me this is super insightful to improve your prompts further).
- **And for me the most important one:** You will want to use chained/connected prompts to achieve better and more complex results. Structured outputs provide the predictability you need for that. This is actually a good segue to my next learning.

### 1 prompt is not enough

When I first started I had the feeling that if my results were not achieved with a single prompt it would mean that I was doing something wrong: this is so not true. What I noticed is that if you want to be consistent with the results you show the user and at the same time make sure the model follows some guidelines on how to perform the task, your system and user prompts can (and I bet they will) get super convoluted, really really fast. I found that I started "overfitting" my prompts at the point where a bunch of instructions started to be ignored by the model, making outputs extremely unpredictable.

One of the things that helped me get through this limitation was letting the model think of the task in steps, feeding on previous outputs. At this point you might have heard that letting the model think on the task in multiple steps is benefitial for the results (train of tought techniques, OpenAI o1 models, [ReAct](https://arxiv.org/abs/2210.03629), they all revolve around this concept). I particularly love the [Alpha Codium paper](https://arxiv.org/pdf/2401.08500) implementation. What I found was that isolating important parts of the process as steps let the prompts (system and user) to be way more focused and specialized. This as consequence produced way better results that compouded at each step.

Latency is defintely a potential problem here, but if you can balance things and/or use async calls to mitigate it, in my opinion it's a small price to pay for very decent results.

### 1 final output is not enough

Same as above, but think about it: LLMs are probabilistic text generation machines. Given that, if you want your users to get good results - on average - you will need multiple attempts to average them out. This might vary from use case to use case, but having this in mind will help you to design systems that will take advantage of having many "thinking heads (or agents)" getting into the best consensus for you.

Agents = system prompt + user prompts + model responses. Mostly the system prompt and the first user prompt will define how an "agent" should behave.

### Cheaper and smaller models are only good for testing

You will always want to use the best model available. It's tempting to delegate some parts of the process to cheaper/smaller models but in the end you will always get better results using the larger/better performant ones. When you use mulitiple steps like mentioned above, one poor response is enough to lead the process into a sub optimal output. Also cheaper models provide worse results and faulty formatted responses.

**Important note:** in the overall scheme of things when testing prompts, these models are actually a great way to have cheap attemps and test how the **system works with your prompts as a whole**, but I would always use the best model available to finish the process and draw the final conclusions.

### Think like a human

The thing I like the most about using multiples prompts (as multiples steps) is that it enables you to add a lot of details and personality to each part of the process. This resembles a lot of with the way we normally do things, making it easier for us to test workflows that are already known but being able to throw mulitple "agents" on it getting faster results.

## Final thoughts

I've been working on this for 8 months now, and I still feel like I'm learning something new every day. I hope you enjoyed it and that it helps you to be more confident on your next steps on building AI apps!

Couple things before you leave:

If you (or anyone you know) is looking for a good and fast way to generate plots, you can check [PlotMaker PRO](https://plotmakerpro.com/). It's a tool that I've been working on for the past 8 months and I'm super excited of the results. It allows you to generate plots with a couple clicks.

I plan to keep sharing more learnings here so if you want to be notified of new posts, consider subscribing to the newsletter!

See you on the next one :)
