---
title: "Chapter 1: How the Wolf Sees"
subtitle: "The mental model you need before you touch a single tool"
label: ch01-how-the-wolf-sees
description: "How LLMs actually work — tokens, context windows, hallucination, and the two pillars of prompting that determine output quality."
---

# Chapter 1: How the Wolf Sees

> *"You don't need to understand how the engine works to drive a car — but if you want to drive it fast, over bad terrain, without breaking down, it helps to know what's under the hood."*

Let's build the mental model that makes everything else in this book work.

Not a computer science lecture. Not a history of neural networks. An operational briefing on how large language models actually work, what they need from you, where they'll fail you, and how to think about them as instruments in a professional environment. By the end of this chapter, you'll have a framework that makes every subsequent tool — Gemini, NotebookLM, AI Studio, Antigravity — immediately more intuitive.

---

## 1.1 The Core Mental Model: IQ With a Memory Problem

Here's the analogy that unlocks everything.

Imagine you're about to receive a brilliant new analyst. This person has read more than any human alive — millions of documents, books, articles, reports, manuals, transcripts, research papers, doctrine publications, contract vehicles, after-action reviews. Their general intelligence is off the charts. They can reason, synthesize, compare, summarize, draft, and structure information with remarkable speed and coherence.

But here's the thing.

On their first day, they walk into your office knowing *nothing* about your mission. They've never heard of the GRASP Model. They don't know your program's history, your client relationships, your current friction points, or the specific format your J7 counterpart expects in a briefing package. They don't know whether you're doing JLLIS entries or running a contractor-led experimentation program. They don't know which six programs just had their Lessons Learned data ingested and which ones are still in collection.

You could ask this analyst a general question — "What are best practices for after-action reviews?" — and get an excellent, well-structured response. But ask them "What patterns are emerging across our last three exercises compared to the previous cycle?" and you'd get a blank stare. Because you haven't briefed them yet.

**That is exactly what a large language model is.**

Extraordinary general intelligence. Zero situational awareness about your specific mission until you provide it.

What happens when you put a brilliant analyst in a room without a briefing? They're frustrating to work with. They're generic. Their outputs could have come from anyone with a library card.

What happens when you brief them properly — give them context, assign a role, define the task, provide the relevant documents? They're elite. The same intelligence, now focused on your actual problem, in your actual operational environment, using your actual terminology.

The entire discipline of *prompting* — which we'll build across this chapter and the next — is about learning to brief your AI the same way you'd brief a high-value analyst who just joined the team.

### What Is the "Knowledge Base"?

The analyst analogy helps explain another question people ask: "How does the model *know* all this stuff?"

It doesn't "know" anything the way a human does. What happened is this: engineers took an enormous corpus of text — we're talking a significant fraction of the human-produced written record — and trained a neural network to predict what comes next in any given sequence of words. Over billions of training examples, the model developed internal representations of language, concepts, relationships, and patterns. When you ask it a question, it's not consulting a database. It's generating a response based on the statistical patterns learned from that training data.

That process produced something remarkable — a system that can reason, generalize, and produce coherent, contextually appropriate text across an almost unlimited range of topics.

But it also produced something with real limitations. The model's knowledge is frozen at a training cutoff date. It can "hallucinate" — generate confident-sounding text that is simply wrong. It has no direct access to current events or real-time information unless you give it tools that provide that access. And it has no memory of your previous conversations unless you explicitly include that context.

More on all of these. But first, the landscape.

---

## 1.2 The Landscape of Models: Three Tiers, Three Jobs

Not all AI models are created equal, and the right tool depends entirely on the task. Here's the framework you need.

### Tier 1: Frontier Models

These are the heavy hitters — the models that sit at the top of every benchmark, trained on the most data with the most compute. As of this writing, the top tier includes:

- **Google Gemini 2.5 Pro** — currently leading the public benchmarks; remarkable long-context capability
- **Anthropic Claude Sonnet / Opus** — exceptional reasoning and writing; the preferred choice for many professional document workflows
- **OpenAI GPT-4o** — widely deployed; strong all-around performance

Frontier models are what you reach for when the task is complex, the stakes are high, or you need the best possible output quality. They're also the most expensive to run via API, though the consumer-facing products (Gemini.google.com, Claude.ai, ChatGPT) are available at flat monthly subscription rates that make cost largely irrelevant for individual use.

For Lukos work, your primary frontier model is **Gemini 2.5 Pro** via gemini.google.com. Everything in Day 1 builds around it.

### Tier 2: Open-Weight Models

These models are released with their weights publicly available — meaning they can be downloaded, run locally, and customized. They're typically smaller and less capable than frontier models, but they have a critical advantage: **they can run on-premises, offline, or on air-gapped networks.**

The most relevant for Lukos:

- **Gemma 3** (Google's open-weight family) — strong performance for its size; covered in Chapter 10
- **Llama 3.x** (Meta's open-weight family) — widely used; good for general tasks at the edge
- **Mistral** — efficient; particularly strong at instruction-following

Open-weight models are your edge capability. When the network isn't available, when classification levels prohibit cloud tools, or when you need to run a model inside a controlled environment, Tier 2 is where you go.

### Tier 3: Specialized/Tuned Models

These are models that have been further trained (fine-tuned) for specific domains or tasks — medical, legal, code generation, embeddings for search. You'll encounter these as you build more sophisticated pipelines, but they're outside the scope of this introductory course.

### The "Best Model" Fallacy

There is no universally best model. There's only the right model for your specific task, constraints, and environment. A 7-billion parameter Gemma model running locally on a laptop is "better" than Gemini 2.5 Pro when you're on a classified network with no cloud access. Claude Opus might be "better" than Gemini for a specific type of document analysis — and worse for another.

The tool that helps you make this call:

### How to Use the Model Arena

**lmarena.ai** (formerly LMSYS Chatbot Arena) is the independent public benchmark for AI models. It works like this: real users submit prompts, and the models compete blind — the user sees two responses without knowing which model produced which, votes for the better one, and the results aggregate into an Elo-style leaderboard.

This is the closest thing to an objective "buying guide" for AI models.

```{admonition} Step-by-Step: Your First Arena Comparison
:class: note

**Goal:** Run a blind comparison of two models on a Lukos-relevant prompt.

1. Open your browser and go to **[lmarena.ai](https://lmarena.ai)**
2. Click **"Arena (battle)"** in the top navigation
3. You'll see a split-screen interface with two anonymous chat windows
4. In the prompt box at the bottom, type:

   *"Write a one-paragraph executive summary of an after-action review for a special operations support exercise. The audience is a senior program manager. Emphasize key lessons and recommended follow-on actions."*

5. Press Enter and wait for both responses
6. Read both outputs. Which one is sharper? More professional? More precisely scoped?
7. Vote for the better one — then click "Reveal Models" to see which was which
8. Click **"Leaderboard"** in the nav to see where both models rank overall

**What you're looking for:** Does the model write to the stated audience? Does it use appropriate professional language? Does it surface *lessons* and *actions* — or generic platitudes?

**Discuss:** What did the winning model do that the other didn't? That difference is what you're paying for (or not) when you choose tools.
```

---

## 1.3 What LLMs Are Good At — And Bad At

### Where AI Earns Its Keep

For Lukos work specifically, AI is well-suited for:

**Summarization** — Condensing a 50-page AAR into a 2-page executive brief. Extracting key themes from a working group transcript. Producing a BLUF paragraph from a dense program evaluation. These are tasks AI performs with remarkable quality at remarkable speed.

**Classification and tagging** — Looking at 200 Lessons Learned entries and categorizing them by theme, severity, function, or recurrence rate. This is one of the highest-ROI applications for the Lukos LL program specifically. A task that might take an analyst three days runs in minutes.

**Drafting** — First-draft briefing memos, talking points, congressional reports, training materials, program summaries. AI produces a credible first draft that a human then reviews, sharpens, and approves. The analyst doesn't disappear. The blank-page problem does.

**Restructuring** — Taking rough working group notes and converting them to formatted OPORD-style documentation. Taking a wall of bullet points and building them into a structured narrative. Taking an informal report and rendering it into a specific template.

**Pattern detection** — Across a large document corpus, AI can surface recurring themes, emerging trends, and anomalous entries that would take human analysts days to find manually. This is the capability that directly amplifies the GRASP Model.

**Comparison** — "Here are four vendor proposals. What are the key differences in approach, risk, and cost?" AI can hold multiple documents in context simultaneously and produce a structured comparative analysis.

### Where AI Gets You Into Trouble

**Mathematics** — Base LLMs are notoriously unreliable at arithmetic and quantitative reasoning. They can appear confident and be wrong. When your analysis requires precise numerical computation, use a calculator. When working in AI Studio, you can give the model a code interpreter tool — that helps. But unassisted arithmetic in a chat window? Verify everything.

**Citation without grounding** — If you ask a model to cite its sources and it doesn't have access to a document set, it will sometimes generate plausible-sounding citations that don't exist. This is a hallucination risk we'll address directly. The solution is grounding: always provide the actual documents and instruct the model to cite only from what you've given it.

**Knowing what it doesn't know** — This is the subtle one. LLMs do not have a reliable sense of their own knowledge boundaries. They will answer a question confidently even when they should say "I don't know." The USSOCOM J7 standard applies: close enough is not close enough. Build verification into your workflow.

**Current events and real-time data** — The model's knowledge has a training cutoff. Anything that happened after that date is unknown to the model unless you provide it. For tasks that require current information, use tools that provide web access (covered in Chapter 9) or provide the relevant documents yourself.

### Hallucination: What It Is and How to Catch It

Here's the blunt version: hallucination is when the model generates text that sounds completely authoritative and is simply not true. It might be a statistic that doesn't exist. A citation to a paper that was never written. A policy detail that's been slightly — or substantially — wrong.

Why does it happen? Because the model was trained to produce *probable* text, not *true* text. Sometimes the most probable next token sequence is one that drifts from accuracy while remaining fluent. The model has no alarm bell that fires when it's wrong.

The Lukos response to hallucination is the same as the Lukos response to any analytical product that hasn't been verified: **you don't publish it until it's been reviewed.**

Practical countermeasures:
- **Provide documents, then ask questions about those documents.** When the model is grounded in a provided corpus, hallucination rates drop substantially.
- **Ask the model to explain its reasoning.** Gaps in the chain of reasoning often surface errors.
- **Instruct the model to say "I don't know" when uncertain.** It won't always comply, but it helps.
- **Cross-reference any specific claim** — statistics, dates, names, policy references — before including them in a deliverable.

The wire brush applies here too. Brutal candor with AI outputs before they leave your hands.

---

## 1.4 Tokens: The Atom of AI

Before we can talk about the two most important concepts in this book, we need to establish a unit of measurement.

A **token** is the basic unit that AI models process. Not a word. Not a character. Something in between — roughly 3-4 characters of English text, or about ¾ of a word. The word "USSOCOM" might be one token, or it might be split into two or three depending on how the model's tokenizer handles it. The word "a" is one token. A period is one token.

Why does this matter? Because AI models operate within a **token budget**. Everything that goes into the model — your instructions, the documents you provide, the conversation history — is measured in tokens. Everything that comes out is measured in tokens. The model's entire operating environment is bounded by this unit.

### A Practical Token Reference

| Content | Approximate Tokens |
|---------|-------------------|
| One page of standard text | ~750 tokens |
| A 10-page AAR | ~7,500 tokens |
| A 50-page program evaluation | ~37,500 tokens |
| A 200-page doctrine publication | ~150,000 tokens |
| An entire year's Lessons Learned corpus (est.) | ~500,000–1,500,000 tokens |

**The Lukos implication:** You now have a unit to think in when you're deciding what to load into an AI session. A 50-page AAR at ~37,500 tokens is manageable. A 500-document corpus at 15 million tokens is not going into a single session — it requires a retrieval strategy (Chapter 4 covers this).

### Token Pricing: The Basic Framework

When you use AI through a consumer app (gemini.google.com, claude.ai), you pay a flat subscription and token costs are abstracted. When you build workflows through an API (AI Studio, Vertex), you pay per token.

The rule: **input tokens are cheap; output tokens cost more.** Reading a 50-page AAR costs a fraction of a cent. Generating a 10-page synthesis document costs more. For Lukos work at the volumes you're operating, the costs are negligible at API rates — but understanding the structure matters when you're building automated workflows that might process thousands of documents.

---

## 1.5 The Token Window — The First Pillar of Prompting

```{admonition} ⭐ This is the most important concept in the chapter.
:class: important

Read this section carefully. Everything else in the book builds on it.
```

Here's the question: if you have a brilliant analyst, how much can they hold in their head at once?

In the real world, the answer is bounded by human working memory — roughly 7±2 chunks of information at any given time, with degradation as that working memory fills up. You brief an analyst on the entire program history, and by the time you get to the current problem, the earliest details have started to fade.

LLMs have an analogous limitation. It's called the **context window** — the maximum amount of text the model can "see" and reason over in a single interaction. Everything outside the window is invisible to the model. Everything inside it is available for reasoning.

### Why the Window Size Changes Everything

For most of AI's history, context windows were small — a few thousand tokens. That meant you could have a conversation, or you could analyze a document, but not both at scale. You couldn't load a 50-page program evaluation and have a sophisticated discussion about it. You couldn't compare three documents simultaneously. You couldn't ask the model to cross-reference patterns across a year's worth of AARs.

Then Gemini happened.

**Gemini 2.5 Pro has a context window of one million tokens.** That's not a typo. One million.

One million tokens is roughly:
- **40 full-length books**
- **An entire program's worth of after-action reviews**
- **Several years of working group transcripts**
- **Your entire Lessons Learned corpus for a single TSOC component**

This is the single biggest differentiator most people aren't talking about. The million-token context window doesn't just let you work with bigger documents. It changes the *category* of questions you can ask. You're no longer limited to "analyze this document." You're now able to ask: "Across every AAR from this program over the last three years, what recurring friction points appear more than twice, and how have they evolved over time?"

That question was not practically answerable with AI twelve months ago. It is now.

### The Two Failure Modes of the Context Window

Understanding the window also means understanding how it fails. There are two:

**Failure Mode 1: The Cliff Edge (Context Overflow)**

When you give the model more text than fits in its window, the oldest content falls off the front. Imagine loading a 100-page document into a model with a 50-page window. The model processes the last 50 pages and has no awareness that the first 50 ever existed. You ask a question that depends on information from page 15 — and the model confidently answers based only on what's in its window.

The fix: be deliberate about what you load and how much. The token reference table above is your planning tool.

**Failure Mode 2: Lost in the Middle**

This is subtler, and more dangerous. Research has demonstrated that LLMs perform better at tasks when the relevant information appears at the *beginning* or *end* of the context window. Information buried in the middle of a very long context is disproportionately likely to be underweighted or missed — even if it's technically within the window.

Imagine you're doing a military land navigation exercise. You can see the start point clearly. You can see the end point clearly. But something in the middle of the terrain gets obscured by trees.

The practical implication for Lukos: when you're asking a model to analyze a large document set, **put your most important context first and your specific question last.** If there's a particular section you need the model to focus on, quote it directly in the prompt rather than relying on the model to find it in a 200-page document.

**The Lukos shorthand:** Where in the prompt the documents sit changes the answer.

---

## 1.6 Token Economics: Reading the Pricing Sheet

You don't need to memorize this, but you should know how to read it.

| Model | Input (per 1M tokens) | Output (per 1M tokens) | Context Window | Best For |
|-------|----------------------|------------------------|----------------|---------|
| **Gemini 2.5 Pro** | ~$1.25 | ~$10.00 | 1M tokens | Complex analysis, long-context |
| **Gemini 2.5 Flash** | ~$0.15 | ~$0.60 | 1M tokens | Fast drafting, classification |
| **Gemini 2.0 Flash** | ~$0.10 | ~$0.40 | 1M tokens | High-volume automation |
| **Gemma 3 (local)** | $0 | $0 | 128K tokens | Edge/offline deployment |
| **Claude Sonnet 3.7** | ~$3.00 | ~$15.00 | 200K tokens | Document writing, reasoning |

*Prices as of writing; always verify at ai.google.dev and anthropic.com/pricing.*

### The Federal-Contracting Angle

For Lukos program managers supporting acquisition: when you're making the case for AI tooling to a client, the ROI calculation is straightforward. If a model costs $0.01 per Lessons Learned document processed and an analyst hour costs $85-$150, the break-even on first-pass AI synthesis is reached after a handful of documents. At 45,000+ records, the economic argument doesn't require sophisticated analysis.

The indirect rate implication: tools that reduce direct labor hours while maintaining output quality affect both the direct cost and the overhead structure. That's an acquisition conversation worth having with your program's contracting officer.

---

## 1.7 Context Engineering — The Second Pillar of Prompting

You now have a model with a million-token context window. The question is: what do you put in it?

This is **context engineering** — and it's the difference between using AI as a search engine and using it as an analyst.

### The Flashlight Theory

Think of the model's attention as a flashlight. The prompt you write determines where the flashlight is pointed. A vague prompt — "summarize this" — is like turning on a flashlight in a dark room and pointing it randomly. You'll illuminate *something*, but probably not the most important thing.

Context engineering is about taking that flashlight and pointing it precisely at what matters.

Here's the structure every effective prompt needs:

**Role** — Who is the model in this interaction? A generic assistant? A retired SF officer with 25 years in unconventional warfare? A USSOCOM J7 analyst specializing in lessons learned synthesis? The role primes the model's entire approach. We'll build full persona libraries in Chapter 2.

**Context** — What does the model need to know to do this task well? The relevant documents. The program background. The audience for the output. The constraints. The format. Context is not overhead — it's the briefing that turns a generic response into a Lukos-quality one.

**Task** — What specifically do you want? Not "analyze this." "Identify all recurring friction points related to interoperability, and for each one: describe the issue, cite the specific AAR or document where it appears, and suggest a follow-on action consistent with USSOCOM J7 guidance."

### The BEFORE and AFTER

Here is the most common Lukos failure mode with AI:

**BEFORE (what most people actually type):**
> *Summarize this AAR.*

What you get: a generic executive summary that could have been written by anyone. Competent. Forgettable. Delivers about 30% of what the model is capable of.

**AFTER (what you'll write by the end of this chapter):**

> *You are a senior Lessons Learned analyst with 15 years of experience supporting USSOCOM components. Your specialty is identifying systemic organizational patterns across exercise AARs.*
>
> *The following document is an after-action review from [Exercise Name], conducted by [Unit], [Date]. The primary audience for your analysis is the J7 program manager and the component LL coordinator.*
>
> *Your task:*
> *1. Identify the top three recurring friction points described in this AAR.*
> *2. For each friction point: state the issue in one sentence, cite the specific page or section of the AAR where it appears, assess whether this appears to be a first-time or recurring issue (note if it aligns with any commonly documented SOF LL themes), and recommend a specific follow-on action.*
> *3. Close with a one-paragraph assessment of this unit's overall learning posture based on the candor and specificity of the AAR content.*
>
> *[Document text follows]*

What you get: an output that reads like it was written by a subject-matter expert. One that a J7 program manager can share directly. One that took the model 30 seconds to produce and the analyst 3 minutes to review.

**That's the gap.** Not the tool — the briefing.

### Why Context Engineering Compounds

Here's the strategic implication. Every prompt you write is an opportunity to build a template. Once you've written the perfect LL synthesis prompt — one that produces exactly the output your J7 counterpart wants — you don't write it again. You save it. You turn it into a Gem (Chapter 2). You give it to your team. You build a library.

Context engineering is the compound interest of AI work. The effort you put in today — writing a precise, reusable prompt — pays dividends every time anyone on the Lukos team uses it.

This is why the appendices in this book exist. We've done a lot of that work for you. Appendix A is the Persona Library. Appendix B is the Gem Library. Appendix C is the Skill Library. They're your starting templates. You refine them based on what works for your specific program.

---

## 1.8 The Two Pillars Together

Here is the complete mental model, rendered as a diagram:

```{mermaid}
flowchart TD
    A["Your Task\n(Lessons Learned synthesis,\nbriefing draft, analysis)"]
    B["🏛️ PILLAR 1\nToken Window\n'How much can the model see?'"]
    C["🏛️ PILLAR 2\nContext Engineering\n'What does the model need to know?'"]
    D["⚙️ The Model\n(Gemini 2.5 Pro)"]
    E["🎯 Output Quality\nElite-grade or generic?"]

    A --> B
    A --> C
    B --> D
    C --> D
    D --> E

    B1["• Load the right documents\n• Sequence: important content first\n• Don't exceed the window"] --> B
    C1["• Assign a Role\n• Provide Context\n• Specify the Task precisely"] --> C

    style B fill:#1a2744,color:#fff,stroke:#2a5298
    style C fill:#1a2744,color:#fff,stroke:#2a5298
    style D fill:#2d1b4e,color:#fff,stroke:#6b3fa0
    style E fill:#1a3a2a,color:#fff,stroke:#2d7a4f
```

### The Two Questions to Ask Before Every AI Interaction

Before you type a single word into a prompt, ask yourself:

**Question 1 (Token Window):** *"What does the model need to see to do this task? Do I have those documents ready? Am I loading them in the right order? Is there too much, or not enough?"*

**Question 2 (Context Engineering):** *"Have I given the model a Role, Context, and Task? Would a new analyst understand exactly what I need from this prompt? Or am I asking a vague question and expecting a precise answer?"*

If you can answer both questions well before you submit, you're prompting at a professional level. It becomes second nature within a week of practice.

The Lukos shorthand: **See clearly, brief precisely.**

---

## 1.9 Hands-On: The Pack Briefing (Group Exercise)

```{admonition} Group Activity — 20 minutes
:class: note

**Goal:** Run a blind model comparison on a Lukos-relevant prompt. Develop team calibration on what "good output" looks like.

**Step 1:** One person opens **[lmarena.ai](https://lmarena.ai)** and clicks **"Arena (battle)"** — put it on the room's shared screen.

**Step 2:** The group agrees on a realistic Lukos task. Suggested prompt:

> *"You are a senior Lessons Learned analyst supporting USSOCOM. You have just finished reviewing an after-action review from a joint exercise focused on interoperability between SOF and conventional forces. Write a one-page executive summary for the J7 program manager. Include: the top three lessons identified, the most significant recommended follow-on action, and an overall assessment of the unit's learning posture. Be direct. Use professional military writing standards."*

**Step 3:** Submit the prompt. Wait for both models to respond (30–60 seconds).

**Step 4:** Read both outputs silently. Then go around the room — each person states one observation about the difference between the two outputs. What did one do that the other didn't?

**Step 5:** Vote for the better output. Then reveal the models.

**Step 6:** Click **"Leaderboard"**. Where do these two models rank? Does the ranking match your vote?

**Debrief questions:**
- Which output would you actually send to a client? Why?
- What would you change about the prompt to improve both outputs?
- What does this tell you about how model choice affects output quality?
```

---

## 1.10 Hands-On: Token Counter (Individual Exercise)

```{admonition} Individual Activity — 10 minutes
:class: note

**Goal:** Develop intuition for document token counts using your own real work.

**Step 1:** Go to **[aistudio.google.com](https://aistudio.google.com)**

**Step 2:** Sign in with your Google account. (If you don't have one, create one at google.com/account.)

**Step 3:** Click **"Create new prompt"** in the left sidebar.

**Step 4:** In the prompt interface, you'll see a text box. Paste a work document you brought with you — an AAR excerpt, a briefing memo, meeting notes, or any professional text. If you didn't bring one, use this sample:

> *"EXERCISE: IRON WOLF 24 | AFTER-ACTION REVIEW SUMMARY | Date: [Date] | Participating Units: [Units] | Exercise Objective: Assess interoperability of SOF and conventional forces during combined joint operations. | Key Observations: (1) Communication between SOF elements and conventional counterparts was hampered by incompatible radio configurations at the beginning of the exercise. This caused a 2-hour delay in the execution of Phase II. (2) The Lessons Learned collection process was initiated on Day 3 rather than Day 1 as planned, resulting in incomplete capture of early-exercise observations. (3) Foreign partner liaison officers were not fully integrated into the planning cycle, limiting their ability to contribute to the operational design. | Recommended Actions: (1) Standardize radio configurations across all participating elements before exercise execution. (2) Brief all unit LL coordinators 48 hours before exercise start. (3) Include foreign partner liaisons in all planning conferences beginning at the STARTEX brief."*

**Step 5:** Look at the upper-right corner of the AI Studio interface. You'll see a token counter — it displays the current token count for your input in real time.

**Step 6:** Note the number. Now delete half the text and watch it drop. Paste it back and watch it rise.

**Step 7:** Answer these questions in the chat or in your notebook:
- How many tokens is your document?
- If Gemini's window is 1 million tokens, how many documents like this could you load simultaneously?
- If your Lessons Learned program processes an average of 10 documents like this per exercise, and you run 30 exercises per year, what's the annual token volume?

**Why this matters:** You're building the instinct that lets you architect AI workflows correctly. Knowing your token volumes tells you whether to load everything into one session or design a retrieval system.
```

---

## 1.11 Hands-On: The Before/After Prompt (Individual Exercise)

```{admonition} Individual Activity — 15 minutes
:class: note

**Goal:** Experience the difference between a vague prompt and a precisely engineered one.

**Step 1:** Go to **[gemini.google.com](https://gemini.google.com)** and sign in.

**Step 2:** Start a new chat.

**Step 3:** Type this prompt and submit it:

> *"Summarize this document."*

Then paste any work text beneath it. Submit and read the result.

**Step 4:** Start a new chat. This time, type this prompt:

> *"You are a senior Lessons Learned analyst with 15 years of USSOCOM support experience. You specialize in identifying systemic patterns across exercise after-action reviews.*
>
> *The following text is an excerpt from an after-action review. The audience for your analysis is a J7 program manager who will use your output to brief the component commander.*
>
> *Your task:*
> *1. Identify the top two lessons described in the text.*
> *2. For each lesson: state it in one direct sentence, note the operational impact, and recommend a specific follow-on action.*
> *3. Rate the overall quality of this AAR on a 1-5 scale based on the specificity and candor of its observations. Justify your rating in two sentences.*
>
> *Document text:*"*

Paste the same text. Submit and read the result.

**Step 5:** Compare the two outputs side by side. Measure the gap.

**Questions to answer:**
- How many additional words would you need to change in the second output before sending it to a client? (The goal is: zero, or close to it.)
- What specific elements of the second prompt produced the most meaningful improvement?
- Identify one thing you would still change about the second output. What prompt instruction would have prevented that?

**The lesson:** You just experienced the entire premise of this chapter in about 10 minutes. That gap — between Output 1 and Output 2 — is what this course closes. Permanently.
```

---

## Field Notes

```{admonition} ⚠️ OPSEC Watch-Out: What Not to Load
:class: warning

**The cardinal rule:** Treat AI cloud tools (gemini.google.com, claude.ai, ChatGPT) as you would an unclassified email system. If you wouldn't email the content to an external collaborator, don't paste it into a commercial AI tool.

**Specifically:**
- No classified information. Ever. At any classification level.
- No PII — names, SSNs, personnel records, medical data.
- No sensitive contract details — proprietary pricing, non-public award information.
- No export-controlled technical data.
- No information that is For Official Use Only (FOUO) if your organization's policy restricts it to internal systems only.

**What is fine:**
- Sanitized AAR excerpts with names, units, and sensitive details removed
- Publicly available doctrine and policy documents
- Generic exercise scenarios and training materials
- Your own professional writing and notes

**The edge solution:** Chapter 10 covers Gemma + LM Studio — running a capable open-weight model locally, with no data leaving your machine. This is the answer for sensitive workflows.

**Federal context:** AI tools used for government-contracted work may have additional restrictions based on your contract vehicle, program classification, and client data handling requirements. When in doubt, check with your program's ISSO or contracting officer.
```

```{admonition} 🔧 Hallucination Mitigation Protocol
:class: tip

Build this into every AI workflow before it produces deliverables:

1. **Ground the model in documents you provide.** Instruct it explicitly: *"Answer only based on the documents I've given you. If you cannot find the information in these documents, say 'I don't have this information in the provided documents.'"*
2. **Verify every specific claim.** Statistics, dates, names, policy references — check them before they go into a deliverable.
3. **Ask for citations.** *"After each key claim, note which document and section it comes from."*
4. **Red-team the output.** After the model produces a synthesis, ask it: *"What are the three things in this output you are least confident about? Why?"*

Not foolproof. But the same rigor you apply to any analytical product that hasn't been verified.
```

---

## Pack Debrief

```{admonition} Pack Debrief — Chapter 1
:class: tip

**BLUF — The five things to walk away with:**

1. **The Analyst Analogy:** LLMs are brilliant generalists with no situational awareness until you brief them. The prompt is the briefing. Better briefing = elite output.

2. **Three Tiers:** Frontier models (Gemini, Claude, GPT-4o) for complex tasks. Open-weight models (Gemma, Llama) for edge deployment. Match the tool to the task.

3. **Token Window (Pillar 1):** Gemini has a 1-million-token window — load a program's worth of AARs and ask cross-cutting questions. Put important content first. Don't bury what matters in the middle.

4. **Context Engineering (Pillar 2):** Every prompt needs Role + Context + Task. "Summarize this" is not a brief. The difference between a vague prompt and a precise one is the difference between 30% and 100% of what the model can do.

5. **Two questions before every prompt:**
   - *"What does the model need to see?"* (Token Window)
   - *"Have I given it Role, Context, and Task?"* (Context Engineering)

**The Lukos Application:** Your 45,000+ Lessons Learned records are waiting. The analytical horsepower to process them faster, find patterns more deeply, and synthesize more precisely is now available. The two pillars are how you unlock it.

**Next:** Chapter 2 — The Field Operative. We take everything in this chapter into gemini.google.com and build your first real Lukos AI tools.
```
