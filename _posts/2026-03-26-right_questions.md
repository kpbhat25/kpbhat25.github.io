---
layout: post
title: "Asking the Right Questions"
date: 2026-03-26
---

# Asking the Right Questions to LLM

*Why knowing what you don't know is harder than knowing what you do*

---

A few years ago, when I attended [random seminars](https://terrytao.wordpress.com/career-advice/attend-talks-and-conferences-even-those-not-directly-related-to-your-work/) in college, people present their research. For the first twenty minutes, I thought I understood the problem; some complex problem they were trying to solve with newer and better algorithms which they published in peer reviewed journal or conferences. Then when someone asked a simple question working in the same field of research or similar domains : *"What happens if a X was something similar to Y , how would the equations change?"* The presenter paused, smiled, and suddenly the entire architecture made sense. That one question unlocked everything.
You appreciated the question which helped you track down why this was done and how everything makes sense.

This is the strange asymmetry of understanding: we often don't know what we're missing until someone shows us. For humans, this is frustrating enough(usually happens when you are working with something complex and you tend to miss a piece because you never thought it that way). For AI systems, it's a fundamental capability we haven't really cracked.

![Asking the Right Questions](/assets/img/blog/mermaid-diagram.svg "Mind Map of the Asking the Right Questions")


## The Missing Piece Problem

Most of the tasks we throw at language models are *underspecified* — like handing someone a puzzle with a piece secretly removed. A math word problem that omits a critical variable. A planning scenario where the initial state is partially hidden. A logic puzzle with a suppressed premise.

The model sees something like: *"Alice is taller than Bob. Who is tallest?"* and must recognize that the chain is broken. It needs to ask: *"How tall is Charlie?"*

This seems trivial. Yet recent work on [QuestBench](https://arxiv.org/abs/2406.00109) reveals something surprising: models that can solve the fully-specified version of a problem often fail at identifying what's missing. On logic and planning tasks, even capable systems only choose the right clarifying question around 40-50% of the time. 

**Solving a well-defined problem and knowing how to make it well-defined are different skills.**

## The Landscape of Not-Knowing

Real queries come in several flavors of incompleteness:

**Underspecification** is the absence of necessary information—the missing premise, the hidden variable. A planning problem with "partially observed initial state" forces the model to probe before it can act.

**Ambiguity** is multiplicity of meaning. When someone asks "Can you bank on that?", are we talking about financial institutions or physical riversides? Clarifying questions collapse these superpositions into single interpretations.

**Overspecification** is the opposite problem: too much noise. Extraneous details that obscure rather than illuminate. The skill here is filtering—knowing which threads to pull and which to ignore.

All three scenarios turn computation into conversation. The model must decide *when* to query and *what* to ask, not merely *how* to answer.

## Games as Testbeds

The classic "20 Questions" has become something of a proving ground for these capabilities. One player holds a secret object; the other must uncover it through yes/no questions. Played optimally, each question halves the remaining possibilities.

Researchers have formalized this as **Strategic Language Search (SLS)**—a two-player game where the questioner faces an adversary who chose the secret. The goal isn't just to win, but to be robust: to guarantee success even against the trickiest opponent. [Game of Thought](https://arxiv.org/abs/2402.10646) applies Nash equilibrium strategies here, ensuring questions work in the worst case, not just the average one.

The results are consistent across benchmarks: simple prompting isn't enough. Greedy heuristics fail. Models need explicit training in information-gathering, whether through algorithms, game theory, or structured planning.

## Planning Under Uncertainty

Several approaches treat question-asking as sequential decision-making:

**Uncertainty of Thoughts (UoT)** explicitly models what the model doesn't know. It simulates possible futures for different candidate questions, estimates their likelihood, and backpropagates information gain to select the optimal probe. Think of it as tree search in the space of possible knowledge. In medical diagnosis and troubleshooting tasks, this improved success rates by ~38% while reducing the number of questions needed.

**Bayesian Experimental Design (BED-LLM)** maintains probabilistic beliefs over unknown variables and selects questions maximizing expected information gain. Each follow-up query becomes an active learning step, chosen to maximally reduce uncertainty.

**KwaiAgents** takes a broader view: building full agent architectures around LLMs, complete with memory, external tools, and planning. The model becomes one component of an interactive system that can browse, retrieve, and iteratively seek information. Through techniques like Meta-Agent Tuning, even 7B parameter models can perform comparably to larger ones in this role.

The common thread: transforming single-step prediction into multi-step decision processes. The LLM becomes not just an answer generator, but an *information-seeking agent*.

## Making Models Ask

How do we actually get systems to exhibit this behavior?

**Prompting and demonstration** remain the first line of defense—prepended instructions, chain-of-thought examples that include clarifying questions. Low cost, but limited robustness.

**Fine-tuning on specialized data** offers more reliability. QuestBench itself provides supervised training signals. Customer support transcripts, tutoring dialogues, any corpus where clarification is necessary—these become training curricula.

**Reinforcement learning** can directly optimize for information gain. Reward models for reducing uncertainty, for successful task completion with minimal queries. Self-play between questioner and answerer agents. Expensive, but potentially transformative.

**Inference-time planning** like BED-LLM's lookahead—simulating possible answers before committing to a question—provides middle ground. No additional training required, but more computation at inference.

**Tool integration** broadens what "asking" means. If a model can query databases or search the web, its information-seeking becomes more powerful. The boundary between asking humans and asking systems blurs.

## What Comes Next

The current benchmarks—logic puzzles, 20 Questions—are synthetic stepping stones. We need richer, multi-turn interaction environments: adaptive tutoring, complex troubleshooting, mathematical proof assistance requiring sustained dialogue.

Human-in-the-loop training could help. Experts still outperform models in knowing *which* question best resolves uncertainty—their demonstrations could guide development.

Better uncertainty estimation would help models recognize their own confusion. Calibrated confidence metrics could trigger querying rather than guessing.

There's also the practical calculus of conversation: too many questions annoy users; too few leave problems unsolved. Optimizing this trade-off—minimizing questions while maximizing success—is its own optimization challenge.

The most promising direction may be integration: game-theoretic robustness combined with simulation-based planning and tool-augmented retrieval. Building agents that can strategically navigate multiple turns of information gathering.

---

There's something almost philosophical here. Asking good questions requires metacognition—thinking about thinking, knowing about knowing. It's the skill that separates surface understanding from deep comprehension. For AI systems to truly assist us in complex, open-ended tasks, they need this capability not as an add-on, but as a core competency.

The research is moving fast. The next generation of assistants might not just answer our questions, but teach us what we should have asked in the first place.

---

**Further Reading:**
- [Attend talks and conferences, even those not directly related to your work](https://terrytao.wordpress.com/career-advice/attend-talks-and-conferences-even-those-not-directly-related-to-your-work/)(Terence Tao)
- [QuestBench: Can Language Models Ask the Right Question?](https://arxiv.org/abs/2406.00109) (Li et al., 2024)
- [Game of Thought: An Introduction to Strategic Language Search](https://arxiv.org/abs/2402.10646) (Cui et al., 2024)
- [Uncertainty of Thoughts: Uncertainty-Aware Planning Enhances Information Seeking in Large Language Models](https://arxiv.org/abs/2402.10646) (Zhang et al., 2024)
- [BED-LLM: Bayesian Experimental Design for Large Language Models](https://arxiv.org/abs/2406.00109) (Xu et al., 2024)
- [KwaiAgents: Generalized Agent Training for Autonomous AI](https://arxiv.org/abs/2402.10646) (Kwai, 2024)