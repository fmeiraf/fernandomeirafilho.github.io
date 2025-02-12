---
title: "Why AI might slow you down: the Paradox of Automation "
date: 2025-02-11T17:55:28+08:00
description: "An overview of AI-driven coding’s speed and its hidden complexities."
tags: ["LLM", "learnings", "AI"]
type: post
weight: 25
showTableOfContents: true
featured: true
---

Probably like a lot of you, I’ve been having a blast using Cursor (or any other AI development workflow). It feels like the future, having complete snippets of code that can go from fixing issues to adding entirely new features.

I won’t even mention the time savings: when you have a vision of where you want to go, you can easily add thousands of AI-generated lines in a couple of hours—sometimes even less. It still feels like magic. **But it can hurt you in the long term.** Let me share why and also something I experienced—so you don’t have to do the same.

Slightly less than a month ago, I shared a [Python library I created](https://github.com/fmeiraf/primeGraph) for organizing and easily executing workflows, mostly focused on AI applications. It took me about eight weeks—just a couple of hours a day—to bring it to a decent level. That would have taken me way longer without AI. Having quick access to LLMs saved me months! But at what cost?

When I faced some major issues or bugs I couldn’t easily solve, I asked AI for help, providing relevant files and even my own suspicions about why things weren’t working. With AI workflows these days, especially if the issues are complex enough, you often need to adjust your prompt a few times (accepting and rejecting diffs if you’re using Cursor, for example) until you get where you want to be.

Following the workflow above, you can easily add dozens or even hundreds of lines per issue. You feel happy because editing three or four prompts is much faster than manually writing those same attempts. The more successes you get, the more you get hooked on the following:

- The incentive to fully read and understand the diffs gets lower and lower as you see more successful outcomes.
- The trade-off between mentally keeping your software’s complexity in your head versus outsourcing it to the model grows bigger and bigger.
- Development quickly turns into a trial-and-error game.
- The overhead to “reconquer” your codebase increases exponentially with the number of AI-generated lines.

Does it feel familiar? **Watch out.** This actually has a name: [**The Paradox of Automation**](https://personalmba.com/paradox-of-automation/). You can check the link for a good explanation on it by Josh Kaufman, but in summary: even though automation aims to reduce the necessity of people in a specific process, it ends up, at the same time, increasing the importance of human skills. Weird? Not exactly. **Automated systems normally produce output at scale, which means that errors are amplified and thus the importance of catching them early in the process.** For that human oversight is more important than ever.

That's is no different when using AI for your coding tasks. You can produce code and add features at scale, and that feels good. But every time you hit a major road block or you need to add a customized feature that LLMs are not able to do, **you are challenged with a huge codebase that you don't know enough how to navigate**. The result of it? **Your further prompts and use of AI on that same codebase will quickly reach extremely diminishing returns** as the quality of your queries are very closely related to how well you are making questions and providing correct guidance so the model can do its best.

Does that mean it’s not worth it? I don’t think so. I would do it all again. But here’s how I completely revamped [primeGraph](https://github.com/fmeiraf/primeGraph)—in just one week—by using LLMs more effectively:

1. **Brainstorm**  
   Use LLMs to help you outline the overall architecture and brainstorm high-level ideas. Explore different paths, evaluate their pros and cons, and see what the LLM suggests. Challenge the LLM on the options you feel are strongest and see if the answers make sense.

   - **Bonus tip:** Ask for a structured plan on applying the ideas you found most appealing. This plan can be a good piece of data to feed the LLM again when starting your project.

2. **Push Back**  
   When using LLMs to solve roadblocks, once presented with a solution, ask how the model thinks it effectively solves the problem.

   - Most of the time, it will either correct itself or revise its first answer—pay attention to what changed and see if it makes sense.
   - I particularly feel this is where the new breed of “thinking models,” like the o1 and o3, DeepSeek, etc., really shine.
   - I also believe it’s good practice to note what went wrong in the first answer and add that back into the prompt, running the same block again instead of just following up. A short, clear conversation is always better than a long back-and-forth.

3. **Be Positioned to Make Good Questions**  
   Your ability to ask clear, insightful questions will dictate the quality and effectiveness of the solutions you get. Losing touch with how your codebase works can lead you to ask more superficial questions, and that can spiral downward.

   - **Bonus tip:** spend some time after a session or adding a feature to query the LLM and learn from the code you generated - if it still doesn't make complete sense to you.

One last crucial observation: you should be aware that the incentive structure of using LLMs can work against you in the long term. If you don’t put yourself in a position to learn from the code you generate and develop a sense of what better code looks like, your continued use of AI will also suffer. The real danger lies in the unknown unknowns.

By the way, if you're creating or working with LLM workflows, I've just updated [**primeGraph**](https://github.com/fmeiraf/primeGraph) to 1.0.0, and I'll soon be sharing more learnings and effective tools that leverage it!
