---
permalink: /posts/en/discussion-chatgpt
date: 2023-01-20
title: 'Discussion about ChatGPT'
categories:
  - work and research
---

This post shows what I had prepared to discuss in the meeting with my research group KnowComp@HKUST. Basically, it first gives the quantitative result of ChatGPT in some projects, summarize ChatGPT's limitation with (my) comments, and my opinion about why/what/how do we do in the next step. The powerpoint version of this post can be found [here](https://dovanquyet.github.io/files/discussion_ChatGPT.pptx)

0. Reference

\[1\] [How does GPT Obtain its Ability?](https://yaofu.notion.site/How-does-GPT-Obtain-its-Ability-Tracing-Emergent-Abilities-of-Language-Models-to-their-Sources-b9a57ac0fcf74f30a1ab9e3e36fa1dc1)
\[2\] [Some remarks on Large Language Models](https://gist.github.com/yoavg/59d174608e92e845c8994ac2e234c8a9)


1. Commonsense reasoning: What ChatGPT can and can't do

- can do (have systematically tested on) Numersense (22/30), CommonsenseQA (10/10?), Pep-3k (28/30), ...
  + with good explanations
- can't do: (few samples from) CSKB Population ~ refuse to answer due to lack of context
  + but, sooner or later, when ChatGPT is updated with RLHF on the topic + external engines, it will solve the task, e.g what happened for the Math dataset, accuracy 10/30 -> 12.5/30, with better calculation


2. (Listed) Problems w/ ChatGPT and (my) comment

- Model belief alignment [1]: Wrong argument and wrong fact.
  + For fact: Need a knowledge graph to firmly store information, instead of relying on updating model parameters. May use a placeholder to train LMs, where the placeholder points to a node in the knowledge graph that stores the time-dependent info. (v)
  + For argument: Either need more data + supervised/RLHF tuning, or 'reading to learn' ~ need the manual = guidance, the RL reasoning model will parse the manual into a policy, then refine the policy by doing some real cases. (^)

- Retrieval from the Internet [1]: It demands the separation of reasoning and knowledge model that if the reasoning model cannot find info in the knowledge model, it should use and treat external knowledge similar to what is from the knowledge model. (^)

- Relating multiple texts to each other [2]: I think LLMs 'know' it, as the representations of aliases of an entity in related texts will be similar
- A notion of time [2]: Yes, but there are existing methods to help LMs unlearn/update info quickly (e.g ? C. Manning's demonstration)
- Numbers and math [2]/Formal reasoning [1]: Using external models (~ programming language) will help
- Rare event/Data hunger [2]: LLMs focus on common cases, thus it may be hard for LLMs to learn rare events. Thus, learning something not common (e.g specialized knowledge) needs a huge effort in data collection. Not yet solved
- Modularity [2]: "If we can modularize and separate the "core language understanding and reasoning" component from the "knowledge" component, we may be able to do much better w.r.t to the data-hunger problem". In short, we build a reasoning model on top of a knowledge/memory model (^)


3. My stance toward ChatGPT (or LLMs + S/RL HF in general)

- Sooner or later, with **RLHF and enough data**, LLMs can learn everything in the world. Maybe it doesn't have reasoning capability, but by remembering data, LLMs can do well on many unseen tasks
- Yet, humans actually can learn in this way (and draw the so-called rule/algorithm to solve the problem more quickly), instead of learning from rules and then apply. Thus, **RLHF and enough data are all LLMs needs**. But they are **costly**. \
--> Thus, if we need to solve a fundamental problem, it should be about **data**.

- Suggested in [1], 'If we can modularize and separate the "core language understanding and reasoning" component from the "knowledge" component, we may be able to do much better w.r.t to the data-hunger problem and the resulting cultural knowledge gaps, we may be able to better deal with and control biases and stereotypes, we may get knowledge-of-knowledge almost "for free".'
- We cannot always have enough data to train LLMs, thus 'reading to learn' is ideal.


4. What to do

- All (^) in section 2. propose to **build a reasoning RL model on top of a knowledge/memory model**.
- Also, **a better knowledge model** is needed, e.g LMs + KGs.
- We focus on the first point.

- Rationale: Why the reasoning model is RL model.
  + For me, SL is experience-based, while RL is rule-based and interaction-based \
    -> RL suits the low-resource regime more than SL. 

- Rationale: Why we need to separate two models.
  + reasons are from section 2. Retrieval from the Internet [1] and Modularity [2]
  + basically, we can focus on the reasoning model, while relieving the burden to remember many things in the system or the knowledge model


5. How to do

- How to separate understanding+reasoning and knowledge+memory: ?
  + Training the model to perform the task, but un-train the model on pieces of info so that it doesn't remember details, just remember given info, how the model performs the reasoning task
  + Or re-train the system, give the reasoning model only 'partial mental states' ~ embedding = knowledge_model(input_string). Embedding will represent partial mental states. The output of the reasoning model is also embeddings, and it lets the knowledge model decode it.

- How to apply K-Lines to train the reasoning model and better store info in the knowledge model? (Sorry, I haven't understood the K-lines theory yet)
  + For reasoning model: use K-lines as objective function/reward model
  + For knowledge model: use K-lines to design the knowledge graph/memory unit \
    ? Is this necessary: not sure, as my stance is that LLMs with enough training can remember all!


6. Conclusion

- Enough data + RLHF will solve almost all tasks, but data hungry
- Separating reasoning and knowledge models likely requires less data to achieve that same performance, thus solve the data hunger
- Rule + A few data = A lot of data!
- To build efficient models, need linguistic theories

