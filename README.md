# LLM: Advancing from Reasoning to Autonomous Reasoning

## Preliminary

Today's group meeting was fantastic and inspired me regarding some questions I've been thinking lately. Here, I compiled these thoughts and included the literature survey that I studied on LLM reasoning. Besides, this also includes some empirical study results that I've conducted recently to validate the effectiveness of contemporary reasoning methods.

<b>Note that: The following discussions focus on LLMs reasoning on the inference stage. Feel free to raise issus and launch the discussion!</b>

-------------------

## Abstract

In recent years, due to significant advancements in LLMs, there's been an observation that LLMs might demonstrate reasoning capabilities. In the subsequent sections, I'll provide some empirical survey detailing <b>how these models acquire reasoning abilities</b>, <b>the evaluation of LLM reasoning</b>, and <b>how to elicit reasoning from LLMs in the inference stage</b>. Finally, <b>I will discuss how to advance LLM from reasoning [[1](https://arxiv.org/pdf/2212.10403.pdf)] to autonomous reasoning [[2](https://www.youtube.com/watch?v=pd0JmT6rYcI)]</b>.

<b>Note that</b>: I'm not the soley contributors of the following context. Many of following information are from my discussion with my friends and the published works ([Jie Huang](https://jeffhj.github.io/) and [Yao Fu](https://franxyao.github.io/)).

<!-- This is a curated list of "LLM Reasoning and SelfEvaluating" research. Read this repository for the latest updates. Feel free to raise issus and launch the discussion! -->
<!--
## NLP Evaluation Survey
- [zhihu](https://zhuanlan.zhihu.com/p/644373658)
-->

## Definiation of Reasoning

Before we discuss, improve, or evaluate LLM reasoning, we should first define: What is reasoning? Here, I prefer to adopt the definition of "informal deductive reasoning" [[1](https://arxiv.org/pdf/2212.10403.pdf), Page2 Section: What is Reasoning?] as our reasoning definition due to its widespread acceptance.

## How to Improve LLM Reasoning

There are four categories of methods to achieve this goal as follows (Click the links below for more reference materials.):
- [Pre-training / Continue Training](https://yaofu.notion.site/Towards-Complex-Reasoning-the-Polaris-of-Large-Language-Models-c2b4a51355b44764975f88e6a42d4e75?pvs=25#cf7f9292f52246429447395e66d59dc7): where one trains a large model on a large dataset, usually scientific literature or <b>code data</b>. 
- [Supervised Finetuning](https://yaofu.notion.site/Towards-Complex-Reasoning-the-Polaris-of-Large-Language-Models-c2b4a51355b44764975f88e6a42d4e75?pvs=25#b82a67cd86e7409da654ac5ec89efb60): where one finetunes the model to follow instructions of complex tasks. 
- [Reinforcement Learning](https://yaofu.notion.site/Towards-Complex-Reasoning-the-Polaris-of-Large-Language-Models-c2b4a51355b44764975f88e6a42d4e75?pvs=25#efc960ee4aac465097f80b3819c48e72): where one uses signals like whether the tasks is fully/ partially finished as the reward. 

The most intriguing point among these methods is that [pre-training codining data can improve the LLM reasoning](https://yaofu.notion.site/Towards-Complex-Reasoning-the-Polaris-of-Large-Language-Models-c2b4a51355b44764975f88e6a42d4e75?pvs=25#c182302830f74fe7a4ddccf6f26195cc). Here are some other empirical studies also verifying this phenomenon ([[3](https://yaofu.notion.site/How-does-GPT-Obtain-its-Ability-Tracing-Emergent-Abilities-of-Language-Models-to-their-Sources-b9a57ac0fcf74f30a1ab9e3e36fa1dc1)],[[4](https://arxiv.org/abs/2210.07128)]), which also arise discussions internally within AI2, and Google. The hypothesis of this phenomenon is that training on code is likely to improve reasoning abilities because:
|  | Reasoning | Coding |
| --- | --- | --- |
| Data format | Chain-of-thought | Line-by-line comments |
| Easy and middle-level task | Step-by-step reasoning | Procedure-oriented programming |
| Hard task | Task decomposition | Object-oriented programming |
- Comments on code are naturally existing chain-of-thought data
- Procedure-oriented programming is similar to solving a task step-by-step. This is applicable to tasks of easy and middle complexity
- Object-oriented programming is similar to decomposing a task into smaller tasks then solve them separately. This is applicable to tasks of higher complexity.

More detail results and analysis about this can refer to [[5](https://yaofu.notion.site/Towards-Complex-Reasoning-the-Polaris-of-Large-Language-Models-c2b4a51355b44764975f88e6a42d4e75?pvs=25#c182302830f74fe7a4ddccf6f26195cc)].


## How to Evaluate LLM Reasoning

I believe you can find the most comprehensive LLM reasoning benchmark [[6](https://github.com/FranxYao/chain-of-thought-hub)]. Thus, instead of introducing the LLM reasoning benchmarks, we will further discuss the challenges of the current reasoning benchmark and metric. Typically, knowledge (can seen as LLM memorization ability) and reasoning ability should be separated when we evaluate:
- Knowledge: whether the model knows things of the world
- Reasoning: whether the model can perform reasoning upon its knowledge.

These two things are not strictly orthogonal to each other because some rules for reasoning can also be viewed as some form of knowledge. Yet these two abilities have clear differences when evaluating: 

- Some datasets focus more on the evaluation of knowledge, such as [[7](https://arxiv.org/abs/2009.03300)], which tests if the model has the knowledge up to college-level.
- Some datasets focus more on the evaluation of reasoning, such as [[8](https://arxiv.org/abs/2210.09261)], which tests if the model has the abilities to solve problems step by step.
- For knowledge, chain-of-thought performs similarly to answer-only (see [[9](https://arxiv.org/abs/2210.11416)] paper)
- For reasoning, chain-of-thought performs significantly better than answer-only (see [[10](https://arxiv.org/abs/2210.11416)] and then [[9](https://arxiv.org/abs/2210.11416)] paper)

In practice, because CoT performs on par with (in knowledge) or better than (in reasoning) answer-only, plus CoT is more user-friendly (because it tells the user thinking process) modern chatbots always deployed with CoT (whatever you ask ChatGPT it tells you a lot of reasoning). Thus, a question arises:

<b>Q: Should we establish a stricter reasoning evaluation metric (that can largely reduce or remove influence of knowledge on reasoning)?</b>


A: Actually, some researchers are working on this, but that metric based on significant assumptions [[11](https://arxiv.org/abs/2307.02477)]. Thus, this might be a direction we can continue to pursue.



## From Reasoning to Autonomous Reasoning

Reasoning is the ability to make inferences, and automated reasoning is concerned with the building of computing mechanisms that automate this process [[12](https://plato.stanford.edu/entries/reasoning-automated/)]. Recently, in the LLM field, researchers see LLMs (Agents in the Figure) as a automated reasoning system and refer to the Reinforcement Learning (RL) paradigm to use them. Specifically, throughout the reasoning process, the LLM is perceived as the agent within this loop.
![0_RGGwVH_AjztI8Jn9](https://github.com/yushengsu-thu/LLM-Advancing-from-Reasoning-to-Autonomous-Reasoning/assets/11704492/5909cc26-9022-4b9c-ba64-244d8286f41a)

It executes corresponding actions based on the plans made, and then, considering the current state and the reward obtained from the environment, it makes the subsequent decision automatically.




---------------------------------------------

## LLM Planning and Sequential Decision Making (Emperical Studies, Evaluation Framework)
- Subbarao Kambhampati [twitter](https://twitter.com/rao2z/status/1716257588768346328), [Google Schlar](https://scholar.google.com/citations?view_op=list_works&hl=en&hl=en&user=yl3L07sAAAAJ&sortby=pubdate&citft=1&email_for_op=yushengsu.thu%40gmail.com)
  - ON THE PLANNING ABILITIES OF LARGE LANGUAGE MODELS(A CRITICAL INVESTIGATION WITH A PROPOSED BENCHMARK) (Benchmark)

## (Self-correct) Can 
- PROMPTAGENT: STRATEGIC PLANNING WITH LANGUAGE MODELS ENABLES EXPERT-LEVEL PROMPT OPTIMIZATION
- LARGE LANGUAGE MODELS AS OPTIMIZERS (Use labeld feedback)
- Reason for Future, Act for Now: A Principled Framework for Autonomous LLM Agents with Provable Sample Efficiency
- CRITIC: LARGE LANGUAGE MODELS CAN SELFCORRECT WITH TOOL-INTERACTIVE CRITIQUING (Use tool feedback)
- HOW FAR ARE LARGE LANGUAGE MODELS FROM AGENTS WITH THEORY-OF-MIND? [1st !!!!]
- Large Language Models are Zero-Shot Reasoners [Only add: let's step by step]

## (Self-correct) Cannot 
- Large Language Models Cannot Self-Correct Reasoning Yet [1st !!!!] 
- GPT-4 Doesn't Know It's Wrong: An Analysis of Iterative Prompting for Reasoning Problems
- Can Large Language Models Really Improve by Self-critiquing Their Own Plans?
- Language Models (Mostly) Know What They Know [1st]
- Benchmarking Foundation Models with Language-Model-as-an-Examiner.
- PandaLM: An Automatic Evaluation Benchmark for LLM Instruction Tuning Optimization
- Large Language Models are not Fair Evaluator

## Talk
- Yann LeCun: [World model, Reasoning, Planning](https://blog.csdn.net/xixiaoyaoww/article/details/129828453)


## Method Design
- EVALUATING LARGE LANGUAGE MODELS AT EVALUATING INSTRUCTION FOLLOWING (Chen Danqi)
- [Eliciting Human Preferences with Language Models](https://arxiv.org/abs/2310.11589)


## Reasoning Survey
- Towards Reasoning in Large Language Models: A Survey (Jie Huang)
- Towards Complex Reasoning: the Polaris of Large Language Models - blog (Yao Fu) 

## Well-Define Reasoning
- Define Reason
  - Self-correct, multiple step reasnong (can only know by the previous step), invlove interaction with the external or not
    
- Reasoning or Reciting? Exploring the Capabilities and Limitations of Language Models Through Counterfactual Tasks (Seen and known != reasoning)

## Reward Strategy (process-based and outcome-based feedback)
- Solving math word problems with process- and outcome-based feedback (fine-tuning)
- Let’s Verify Step by Step (refer to the anaysls and methods here)

## Conclusion
- zero-shot chain-of-thought (works because of involving code pre-training, )
- self-correct (doexn't work)
- RL reward (works but has some limitation)
    - TOOLCHAIN : EFFICIENT ACTION SPACE NAVIGATION IN LARGE LANGUAGE MODELS WITH A SEARCH (well illistrate the limitation)

-----------------------------------

## RL Reward and Limiation

- TOOLCHAIN : EFFICIENT ACTION SPACE NAVIGATION IN LARGE LANGUAGE MODELS WITH A SEARCH



[1] [Towards Reasoning in Large Language Models: A Survey](https://arxiv.org/pdf/2212.10403.pdf)

[2] [From Machine Learning to Autonomous Intelligence – AI-Talk by Prof. Dr. Yann LeCun](https://www.youtube.com/watch?v=pd0JmT6rYcI)

[3] [How does GPT Obtain its Ability? Tracing Emergent Abilities of Language Models to their Sources](https://yaofu.notion.site/How-does-GPT-Obtain-its-Ability-Tracing-Emergent-Abilities-of-Language-Models-to-their-Sources-b9a57ac0fcf74f30a1ab9e3e36fa1dc1)

[4] [Language Models of Code are Few-Shot Commonsense Learners](https://arxiv.org/abs/2210.07128)

[5] [Towards Complex Reasoning: the Polaris of Large Language Models](https://yaofu.notion.site/Towards-Complex-Reasoning-the-Polaris-of-Large-Language-Models-c2b4a51355b44764975f88e6a42d4e75)

[6] [Chain-of-Thought Hub: Measuring LLMs' Reasoning Performance](https://github.com/FranxYao/chain-of-thought-hub)

[7] [Measuring Massive Multitask Language Understanding](https://arxiv.org/abs/2009.03300)

[8] [Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them](https://arxiv.org/abs/2210.09261)

[9] [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

[10] [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)

[11] [Reasoning or Reciting? Exploring the Capabilities and Limitations of Language Models Through Counterfactual Tasks](https://arxiv.org/abs/2307.02477)

[12] [Stanford Encyclopedia of Philosophy](https://plato.stanford.edu/entries/reasoning-automated/)
