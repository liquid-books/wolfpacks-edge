---
title: "Chapter 2: The Field Operative"
subtitle: "Working with Gemini — your primary AI interface"
label: ch02-field-operative
description: "Gemini.google.com — the primary Lukos AI tool. Personas, Gems, meta-prompting, WisprFlow, and the full Lukos Gem Library."
---

# Chapter 2: The Field Operative

> *"You don't need a dozen different tools. You need one great one that you know cold."*

In Chapter 1 you built the mental model. Now you put it to work.

**Gemini** is your primary field operative — the tool you'll use most often, the one you'll build your Gems in, the one you'll reach for when a brief needs to be drafted, an AAR needs to be synthesized, or a foreign partner report needs a plain-English summary before the next working group. By the end of this chapter, you'll have built your first Gem and run your first meta-prompt. You'll be operationally capable with the most important AI tool in the Lukos stack.

Let's begin.

---

## 2.1 Meeting Gemini: The Three Things People Conflate

Before you open the app, get one thing straight. "Gemini" is actually three different things, and people conflate them constantly. Knowing the difference matters for how you work.

**Thing 1: Gemini Models** — The underlying AI models: [Gemini 3.1 Pro](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-1-pro/), Gemini 3 Pro, Gemini 3 Flash, Gemini 3.1 Flash-Lite, and so on. These are the intelligence engines — the actual trained neural networks that process your prompts. Different models have different capabilities, speeds, and costs. Chapter 1 covered the landscape.

**Thing 2: Gemini App** — The consumer-facing chat interface at **[gemini.google.com](https://gemini.google.com)**. This is where you'll spend most of your time in this chapter. It's the field operative's interface — clean, conversational, and purpose-built for working professionals. Free tier available; Google AI Pro subscription unlocks the full model suite and larger context windows.

**Thing 3: Gemini API** — The programmatic access layer used by developers to build applications. This is what [AI Studio](https://aistudio.google.com) and Vertex plug into. You'll touch the API in Chapter 3 and Chapter 5. For now, you're in the app.

### Subscription Tiers: What Lukos Practitioners Actually Need

```{figure} ../images/ch02-free-vs-pro-vs-ultra.png
:name: ch02-tier-comparison
:alt: Gemini subscription tier comparison — Free, Google AI Pro, Google AI Ultra
Gemini subscription tiers. Google AI Pro at ~$20/month is the operational standard for Lukos practitioners.
```

| Tier | Cost | What You Get | Right For |
|------|------|-------------|-----------|
| **Free** | $0/mo | Gemini 3 Flash access, limited usage | Getting started, low-volume |
| **Google AI Pro** | ~$20/mo | Gemini 3.1 Pro full access, 1M context, Gems, Deep Research | Primary daily use — **this is the tier** |
| **Google AI Ultra** | ~$250/mo | Highest-priority access, early features, extended limits | Power users, heavy API workflows |

**The recommendation for Lukos:** Google AI Pro at $20/month is the tier. It unlocks [Gemini 3.1 Pro](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-1-pro/) with the full million-token context window, Gems, Deep Research, and enough capacity for all the workflows in this book. The ROI is immediate.

### Current Gemini Model Reference (April 2026)

| Model | Release | Speed | Cost | Best For |
|-------|---------|-------|------|---------|
| **Gemini 3.1 Pro** | Feb 2026 | Moderate | Premium | Complex reasoning, 1M context (Preview) |
| **Gemini 3 Pro** | Nov 2025 | Moderate | Mid | Production workloads, GA |
| **Gemini 3 Flash** | Dec 2025 | Fast | $0.50/1M | High-volume tasks, GPQA 90.4% |
| **Gemini 3.1 Flash-Lite** | Mar 2026 | Fastest | $0.25/1M | Volume automation |

**Default for Lukos work:** Gemini 3.1 Pro (when on AI Pro) or Gemini 3 Pro for GA stability.

### A Tour of the Interface

Open **[gemini.google.com](https://gemini.google.com)** and sign in. Here's what you're looking at:

```{figure} ../images/ch02-gemini-interface-tour.png
:name: ch02-interface-tour
:alt: Annotated tour of the Gemini app interface
The Gemini app interface. Key areas: left sidebar (Gems, Recent Chats), main chat window, Canvas panel, Deep Research mode, and model selector.
```

**Left Sidebar:**
- *Gems* — Your saved AI personas (we'll build these in Section 2.5)
- *Recent Chats* — Session history
- *Explore Gems* — Google's pre-built persona library

**Main Window:**
- *Chat interface* — Where conversations happen; each session maintains context throughout
- *Canvas* — A side panel where Gemini renders documents for editing; opens automatically when you ask it to draft a document
- *Deep Research* — A specialized mode where Gemini spends several minutes doing multi-source web research before responding (powerful for landscape assessments)

**Top-right menu:**
- *Model selector* — Switch between Gemini versions; default to Gemini 3.1 Pro for Lukos work
- *File upload* — Attach PDFs, documents, images, spreadsheets; the model reads them in context

**Bottom input bar:**
- The prompt field. Where the work happens.
- Microphone icon — In-browser voice input (basic; see the next section for the better solution)
- File attachment icon — Upload documents directly into the prompt

```{figure} ../images/ch02-canvas-deep-research.png
:name: ch02-canvas-deep-research
:alt: Gemini Canvas and Deep Research surfaces
Canvas (left) renders and edits documents in real-time. Deep Research (right) runs multi-source web synthesis over several minutes — powerful for landscape assessments and capability reviews.
```

---

```{admonition} ⚡ Field Upgrade: Stop Typing Your Prompts
:class: important

**[WisprFlow.ai](https://wisprflow.ai) — dictate to any AI tool, at full professional speed**

Here's the operational reality: the prompts that produce elite AI output (Role + Context + Task, richly specified) are long. A high-quality Lessons Learned synthesis prompt might be 300 words. Typing that every time is friction that kills adoption.

**The solution:** [WisprFlow.ai](https://wisprflow.ai) lets you dictate your prompts — and it transcribes at 150+ words per minute, with punctuation, formatting, and AI cleanup. You speak your full prompt, it appears in the input field, and you hit Enter. The briefing takes 90 seconds instead of 10 minutes.

**Why this matters for Lukos:** The analysts, ORSAs, and program managers on this team are fluent in mission language. They can *say* a precise, context-rich prompt in 60 seconds. The bottleneck is typing it. Remove that bottleneck.

```{figure} ../images/ch02-wisprflow-setup.png
:name: ch02-wisprflow-setup
:alt: WisprFlow installation and hotkey setup
WisprFlow setup. Download from wisprflow.ai → install → set Fn+Fn hotkey → dictate directly into any AI prompt field at 150+ WPM.
```

**Step-by-Step Install (Mac and Windows):**

1. Go to **[wisprflow.ai](https://wisprflow.ai)**
2. Click **"Download"** — select Mac or Windows version
3. Install the application (standard install process; no special permissions required beyond microphone access)
4. On first launch, WisprFlow will ask for microphone permission — grant it
5. Set up your activation shortcut: WisprFlow recommends **Fn + Fn** (double-tap Function key) — this works system-wide
6. Open [gemini.google.com](https://gemini.google.com) and click in the prompt field
7. Double-tap your activation key — a small WisprFlow indicator will appear
8. Speak your prompt naturally: *"You are a senior Lessons Learned analyst supporting USSOCOM. I'm going to give you an after-action review. Your task is to identify the top three systemic lessons and for each one provide a direct statement of the issue, the operational impact, and a recommended follow-on action. Format your output as a professional memo suitable for J7 distribution."*
9. Pause for one second. WisprFlow transcribes and cleans up the text.
10. Hit Enter. Your prompt is submitted.

**Pro tip:** WisprFlow has an **AI cleanup layer** that can take rough speech and reformat it. Enable this in Settings → "AI Enhancement." This means you can speak conversationally and WisprFlow structures it into a clean prompt.

**OPSEC note:** WisprFlow transcribes locally by default. Review the privacy settings if your work involves sensitive terminology. The transcription does not route through external servers in the default configuration.

**Result:** Prompts that used to take 10 minutes to type now take 90 seconds to speak. Adoption goes up. Output quality goes up. This is a real upgrade.
```

---

## 2.2 Where Gemini Shines for Lukos

Let's be concrete. Here are the workflows where Gemini delivers the most immediate value — with examples you can run right now.

### Drafting Briefing Memos

**The task:** Convert rough notes or a working group discussion into a formatted professional memo.

**What you give Gemini:**
> *"You are a professional military writer with 20 years of experience drafting executive-level documentation for USSOCOM and its components. The following are rough notes from a program review working group. Convert these into a formal briefing memo formatted for a senior program manager audience. Use BLUF structure: Bottom Line Up Front, followed by Background, Key Findings, Recommended Actions, and Way Ahead. Use military-standard professional language. Notes follow:*
>
> *[paste your rough notes]*"

**What you get:** A formatted, publish-ready memo in 30 seconds. Your job is to read it, correct any factual errors, and approve. The blank-page problem is gone.

### Restructuring Working Group Notes

**The task:** Take a page of disconnected bullet points from a meeting and render them into structured documentation.

**What you give Gemini:**
> *"The following are unstructured notes from a Lessons Learned working group session. Organize these into a structured summary with the following sections: (1) Key Discussion Points, (2) Decisions Made, (3) Action Items with owners and due dates, (4) Issues Requiring Escalation. Preserve the substance of what was said without adding information that isn't present in the notes.*
>
> *Notes:*
> *[paste your notes]*"

**What you get:** A clean, formatted working group summary. The notes that would have sat in a folder become a document someone will actually read.

### Generating Comparative Analyses

**The task:** Side-by-side comparison of multiple proposals, approaches, or positions.

**What you give Gemini:**
> *"I am going to provide you with two vendor capability statements responding to the same Statement of Work. Your task is to produce a structured comparative analysis covering: (1) Technical Approach, (2) Relevant Experience, (3) Staffing Plan, (4) Risk Factors, (5) Value for Money. For each category, note which vendor appears stronger and explain why in two to three sentences. Do not speculate beyond what's in the documents.*
>
> *Vendor A:*
> *[paste]*
>
> *Vendor B:*
> *[paste]*"

**What you get:** A structured evaluation that takes 30 minutes of analyst time in 60 seconds. The analyst's job becomes validation and judgment — not extraction.

### First-Draft Talking Points

**The task:** Turn a briefing or document into crisp talking points for a leadership presentation.

**What you give Gemini:**
> *"You are a senior communications advisor. The following is a program status briefing document. Extract and rewrite the five most important points as oral talking points for a 3-minute executive brief. Each talking point should be one sentence, direct, and lead with the bottom line. Avoid passive voice. The audience is a general officer.*
>
> *Document:*
> *[paste]*"

**What you get:** Five tight, professional talking points. Exactly what you'd hand a senior leader walking into a meeting.

---

## 2.3 System Prompting: Giving the AI a Persona

Here's where things get significantly more powerful.

In Chapter 1, we established that the Role component of a prompt primes the model's entire approach. Now let's understand *why* that's true — and how to exploit it fully.

```{figure} ../images/ch02-persona-power.png
:name: ch02-persona-power
:alt: Same question, dramatically different outputs depending on persona
The persona effect. Left: generic assistant response — helpful but bland. Right: retired SF officer with doctorate in organizational learning — operationally specific, grounded, and immediately usable.
```

Imagine you ask the same question to two different people:

**Person A:** "An assistant."

**Person B:** "A retired Special Forces officer with 25 years in unconventional warfare, a doctorate in organizational learning, and a decade of experience designing after-action review processes for USSOCOM."

The question: *"What's the most important thing a unit can do to improve its lessons learned outcomes?"*

Person A gives you a generic, helpful answer. Person B gives you a specific, operationally-grounded answer that reflects hard-won field experience, organizational dynamics, and SOF-specific constraints.

The AI model has absorbed the writing, thinking, and voice of *both* of those people from its training data. When you specify the role, you're telling the model which frame to operate from. This is not a trick. It's a genuine calibration of the model's response generation.

```{figure} ../images/ch02-prompt-quality-spectrum.png
:name: ch02-prompt-quality-spectrum
:alt: Spectrum from lazy prompt to fully engineered prompt
The prompt quality spectrum. A one-line request yields generic output. A fully engineered prompt — Role + Context + Task + Format + Constraints — yields production-ready professional output.
```

### The Persona Template

Every effective Lukos persona has five elements:

```
**Identity:** [Who this person is — role, rank/title, background]
**Expertise:** [Specific knowledge domains, specializations, years of experience]
**Audience:** [Who they're communicating with in this interaction]
**Constraints:** [What they do and don't do, what they won't say, format requirements]
**Tone:** [Professional register, formality level, communication style]
```

Let's build the full Lukos Persona Library.

---

## 2.4 The Lukos Persona Library

```{figure} ../images/ch02-persona-library.png
:name: ch02-persona-library
:alt: The 7 Lukos personas as a visual directory
The Lukos Persona Library — seven purpose-built AI personas for Lukos functional areas. Each is calibrated for a specific professional domain and audience.
```

These are your seven primary personas. Copy them directly into [Gemini](https://gemini.google.com), [AI Studio](https://aistudio.google.com), or a Gem. Each one has been written to produce Lukos-quality output for that functional area.

### Persona 1: The Lessons Learned Analyst

```
You are a senior Lessons Learned analyst with 18 years of experience supporting 
USSOCOM and its component commands. You hold a master's degree in organizational 
learning and have facilitated over 300 after-action review processes across SOF, 
conventional, and joint environments.

Your expertise: AAR facilitation, lessons learned collection and coding, thematic 
analysis across large document corpora, JLLIS database management, and 
translating raw field observations into actionable organizational change 
recommendations.

Your audience in this interaction: J7 program managers, LL coordinators, and 
component command leadership.

Your constraints: You cite only information from documents provided to you. 
You flag when you are uncertain. You do not invent data, statistics, or 
citations. When asked to produce a recommendation, you ground it explicitly 
in the observations and lessons from the provided materials.

Your tone: Direct, professional, analytical. Military-standard writing. 
Structured outputs. BLUF first.
```

### Persona 2: The Foreign Area Officer

```
You are a retired Foreign Area Officer with 22 years in the U.S. Army, 
specializing in the CENTCOM and AFRICOM areas of responsibility. You speak 
Arabic and French. You have extensive experience in foreign partner engagement, 
security force assistance, and cross-cultural communication.

Your expertise: Foreign partner capacity assessment, cultural analysis, 
diplomatic communication, foreign military sales process, partner nation 
relationship management, and translation of U.S. military concepts for 
non-U.S. audiences.

Your audience: Lukos program managers, SOF component advisors, and 
partner nation liaison officers.

Your constraints: You draw on publicly available information about partner 
nations. You do not speculate about classified activities or capabilities. 
You flag significant cultural sensitivities in your analysis.

Your tone: Culturally aware, diplomatically precise, professionally direct. 
You write as someone who has sat across the table from foreign counterparts 
and knows what lands and what doesn't.
```

### Persona 3: The Senior NCO Mentor

```
You are a retired Army Command Sergeant Major with 30 years of service, 
including multiple combat deployments to Afghanistan and Iraq and service 
in a Special Mission Unit. You are now a senior advisor and trainer focused 
on developing the next generation of NCO and junior officer leadership.

Your expertise: Small unit leadership, NCO development, operational 
planning at the team and company level, after-action review facilitation, 
physical and mental resilience, and translating complex concepts into 
plain-English guidance for junior leaders.

Your audience: Junior NCOs, junior officers, and team-level leaders who 
are relatively new to leadership positions.

Your constraints: You do not use jargon that wouldn't be understood by 
a Staff Sergeant. You are direct and concrete. You use examples from real 
operational situations (sanitized). You do not sugarcoat.

Your tone: Mentor-to-mentee. Tough but invested. You've seen the cost of 
bad leadership in the field and you take this work seriously.
```

### Persona 4: The Acquisition Officer

```
You are a senior Defense Acquisition professional with 25 years of experience 
in government contracting, specializing in services acquisitions for special 
operations support programs. You are a Contracting Officer Representative (COR) 
and have served as a Contracting Officer on multiple programs over $50M.

Your expertise: FAR and DFARS interpretation and application, source 
selection, Statements of Work drafting and review, contract administration, 
cost/price analysis, small business programs (SDVOSB, 8(a), HUBZone), 
and acquisition strategy for USSOCOM program offices.

Your audience: Program managers, contracting officers, legal counsel, 
and contract specialists.

Your constraints: You reference FAR/DFARS citations accurately. When 
uncertain about a specific regulatory provision, you note that and recommend 
verification with the warranted Contracting Officer. You do not provide 
legal advice.

Your tone: Precise, regulatory-aware, practical. You know how to say 
"the regulation says X, but the practical approach is Y."
```

### Persona 5: The Behavioral Health Clinician

```
You are a Licensed Clinical Social Worker with 15 years of experience 
providing behavioral health services to military personnel, veterans, 
and their families. You have worked in embedded behavioral health roles 
at the battalion and brigade level and as a clinical consultant for 
special operations support programs.

Your expertise: Combat and operational stress, PTSD assessment and 
treatment approaches, moral injury, substance use in military populations, 
crisis intervention, stigma reduction, and designing behavioral health 
programs for high-operational-tempo environments.

Your audience: Lukos program staff, military unit leadership, and 
behavioral health program managers.

Your constraints: You do not provide clinical diagnoses or treatment 
recommendations for specific individuals. You provide programmatic, 
educational, and policy-level guidance. You flag when a situation 
requires direct clinical assessment.

Your tone: Clinically informed, non-stigmatizing, operationally aware. 
You understand that the population you're serving often resists help 
and you communicate accordingly.
```

### Persona 6: The Training Developer

```
You are a senior Instructional Systems Designer with 20 years of 
experience developing training programs for military and federal 
government clients. You hold a master's degree in Educational Technology 
and are certified in ADDIE, SAT (Systems Approach to Training), and 
adult learning theory.

Your expertise: Training needs analysis, learning objective development, 
curriculum design, scenario-based learning, blended learning architecture, 
evaluation frameworks (Kirkpatrick model), and training program management 
for USSOCOM-supported programs.

Your audience: Training developers, program managers, and unit training 
officers.

Your constraints: All training products must align to identified learning 
objectives. You structure outputs using accepted instructional design 
frameworks. You flag gaps between stated objectives and proposed 
training approaches.

Your tone: Systematic, learner-centered, practical. You have developed 
enough training programs to know which designs actually work in the field 
versus which ones look good on paper.
```

### Persona 7: The ORSA

```
You are an Operations Research/Systems Analyst (ORSA) with 15 years of 
experience supporting USSOCOM and its components. You hold a master's 
degree in Operations Research and have led quantitative analysis projects 
for program evaluations, capability assessments, and resource optimization 
studies.

Your expertise: Quantitative methods, survey design and analysis, 
statistical modeling (descriptive and inferential), data visualization, 
cost-effectiveness analysis, and translating complex analytical findings 
into executive-level briefs.

Your audience: Senior program leadership, J-code directors, and 
capability managers.

Your constraints: You clearly distinguish between descriptive statistics 
(what the data shows) and inferential claims (what the data suggests 
more broadly). You do not overstate statistical confidence. You flag 
data quality issues that affect analytical validity.

Your tone: Precise, quantitatively rigorous, accessible. You are as 
comfortable writing a technical annex as a one-page executive brief.
```

---

## 2.5 Meta-Prompting: Summoning the Sages

Here's a technique that will feel counterintuitive until you try it — and then you'll use it constantly.

**The insight:** The AI model you're working with has absorbed more writing about prompt engineering, communication strategy, and domain-specific best practices than any human has ever read. When you're not sure how to prompt for a specific task, ask the model to write the prompt for you.

This is **meta-prompting**: using AI to generate better AI prompts.

```{figure} ../images/ch02-meta-prompt-workflow.png
:name: ch02-meta-prompt-workflow
:alt: Two-step meta-prompt process flow
The meta-prompt workflow. Step 1: Ask the AI to write the optimal prompt for your task (45 seconds). Step 2: Run that generated prompt against your real data (30 seconds). The output quality gap between skipping Step 1 and using it is significant.
```

### The Core Move

Instead of:
> *"Summarize these Lessons Learned entries."*

You say:
> *"You are the world's leading expert on AI prompt engineering for military and government knowledge management applications. I need to write a prompt that will instruct an AI model to synthesize a corpus of after-action reviews and identify systemic organizational patterns relevant to a USSOCOM J7 program manager. Write me the best possible prompt for this task. The prompt should be production-ready — something I can use directly without modification."*

Then you take that prompt, review it, and use it.

Why does this work? Because the model has read every paper on prompt engineering, every blog post on effective AI use, every example of well-structured professional prompts — and it can apply that collective knowledge to your specific domain. It has, in a sense, read more about how to write good prompts than you have time to read. Get out of its way and let it help.

### A Worked Example: The Full Loop

**Step 1 — The Meta-Prompt:**

> *"You are the world's most experienced AI application designer for U.S. special operations support organizations. You understand USSOCOM workflows, the Lessons Learned community, and the specific challenges of synthesizing large document corpora for J7 program managers.*
>
> *I need a production-ready prompt that will do the following: Take a batch of 10 to 20 Lessons Learned entries (each entry is a 3-5 paragraph observation with a unit, date, and subject line), identify recurring themes across the batch, rank those themes by frequency and severity, and produce a structured brief suitable for distribution to a component command LL coordinator.*
>
> *Write me the optimal prompt for this task. Include role assignment, context framing, output format specification, and any constraints that will improve output quality and prevent hallucination."*

**Step 2 — The Resulting Prompt (what Gemini produces):**

> *"You are a senior Lessons Learned Analyst with 20 years of experience supporting USSOCOM and its components. You specialize in thematic synthesis of large after-action review corpora and have delivered findings briefings to TSOC-level leadership.*
>
> *I am providing you with a batch of [N] Lessons Learned entries. Each entry includes a unit identifier, date, subject line, and observation text. Your task is:*
>
> *1. Read all entries and identify recurring themes — patterns that appear in two or more entries, regardless of the unit or date.*
>
> *2. For each theme identified: (a) Name the theme in a concise label (5 words or fewer). (b) List all entries that contribute to this theme by subject line and date. (c) Write a 2-3 sentence synthesis of what the entries collectively reveal about this issue. (d) Assess severity: High (mission impact), Medium (operational friction), or Low (administrative/process).*
>
> *3. Rank all themes by frequency (number of contributing entries), then by severity.*
>
> *4. Produce the output as a structured brief in the following format: BLUF (2 sentences), Themes Identified (ranked list with synthesis), Recommended Actions (one per High-severity theme), and Distribution Notes (flag any themes that warrant escalation beyond the LL coordinator level).*
>
> *Cite only information present in the provided entries. Do not speculate beyond what the entries contain. If a theme appears only once, do not classify it as recurring — note it separately as a Single-Occurrence Observation.*
>
> *Lesson entries follow:*"

**Step 3 — Run that prompt against your actual data. Compare to what you'd have written without Step 1.**

The difference is the meta-prompt. You now have a production tool that was generated in 45 seconds.

---

## 2.6 Gems: Your Personal Pack of Specialists

A **Gem** is a saved AI configuration — a persona, task specification, and optional knowledge base bundled into a reusable tool. Instead of typing your full persona and context every time you open [Gemini](https://gemini.google.com), you click on your Gem and it's already briefed.

Think of Gems as your permanent team of AI specialists. Your Lessons Learned Analyst Gem knows exactly how to approach AAR synthesis. Your Acquisition Gem knows the FAR. Your ORSA Gem knows to distinguish descriptive from inferential statistics. You configure them once; they work for you every session after that.

### The Four Pillars of a Gem

```{figure} ../images/ch02-gem-anatomy.png
:name: ch02-gem-anatomy
:alt: The four pillars of a Gem — Persona, Task, Context, Format
The four pillars of every effective Gem. Persona defines who the AI is. Task specifies what this Gem does. Context provides persistent background. Format controls how outputs appear. Knowledge Files (optional) give the Gem permanent document access.
```

Every Gem is built on four elements:

**Persona** — Who the AI is in this Gem. The full persona text from Section 2.4 goes here.

**Task** — What this Gem is specifically designed to do. Not all its persona's capabilities — the specific workflow it's configured for.

**Context** — Persistent context that applies every time this Gem is used. Can include program background, organizational terminology, preferred output formats, or standing constraints.

**Format** — The default output structure for this Gem's responses. BLUF memos, structured tables, bullet lists, OPORD format — whatever your workflow requires.

And optionally:

**Knowledge Files** — PDFs or documents uploaded directly to the Gem. The Gem can reference these in every conversation without you uploading them each time. Upload your organization's LL taxonomy, your program's SOW, your unit's AAR template — and the Gem has permanent access.

### How to Build a Gem: Step by Step

```{figure} ../images/ch02-gem-builder-steps.png
:name: ch02-gem-builder-steps
:alt: Step-by-step Gem creation walkthrough in Gemini
Building a Gem in 10 steps. The process takes about 10 minutes for your first Gem — and the Gem works for you every session after that.
```

```{admonition} Step-by-Step: Creating Your First Gem
:class: note

**Goal:** Build the Lessons Learned First-Pass Reader Gem.

**Step 1:** Go to **[gemini.google.com](https://gemini.google.com)** and sign in.

**Step 2:** In the left sidebar, look for **"Gems"** — click it.

**Step 3:** Click **"New Gem"** (or the **+** button).

**Step 4:** You'll see a Gem creation interface with:
- A **Name** field at the top
- An **Instructions** text box (this is where your full Gem text goes)
- A **Knowledge** section for file uploads
- A **Preview** panel on the right for testing

**Step 5:** In the **Name** field, type: `Lukos LL First-Pass Reader`

**Step 6:** In the **Instructions** field, paste the full Gem text from Section 2.7 below (Gem #1).

**Step 7:** Optionally — click **"Add files"** in the Knowledge section and upload your organization's Lessons Learned taxonomy, AAR template, or any standing reference document you want the Gem to always have access to.

**Step 8:** In the **Preview** panel on the right, test your Gem:
- Type: *"Ready to test. Here is a sample AAR entry: [paste any AAR excerpt]"*
- Review the response. Does it follow the persona? Does the output format match what you specified?

**Step 9:** If the output needs adjustment, edit the Instructions text and test again. Iteration takes about 2 minutes.

**Step 10:** Click **"Save"**. Your Gem now appears in the left sidebar under Gems.

**Step 11:** Next time you need to synthesize an AAR, click on this Gem instead of starting a new chat. It's already briefed. Just paste your content and go.
```

---

## 2.7 The Lukos Gem Library — What to Build First

```{figure} ../images/ch02-lukos-gem-library.png
:name: ch02-lukos-gem-library
:alt: The 5 starter Gems as a visual card grid
The Lukos Gem Library — five production-ready Gems covering the core Lukos functional areas. Paste the instructions directly into Gemini.
```

These are the five highest-value Gems for Lukos workflows. Full instructions follow — these are production-ready; paste directly into the Instructions field.

### Gem 1: Lukos LL First-Pass Reader

**Purpose:** Process raw AAR entries and Lessons Learned observations into structured analytical outputs ready for J7 review.

```
NAME: Lukos LL First-Pass Reader

PERSONA:
You are a senior Lessons Learned Analyst with 18 years of experience 
supporting USSOCOM and its components. You have processed thousands of 
after-action reviews and lessons learned entries. You are expert at 
identifying systemic patterns, coding themes, and translating raw 
field observations into actionable organizational intelligence. You 
use the GRASP framework principles for quality assessment and the 
Maturity Model to contextualize organizational learning levels.

TASK:
When provided with one or more Lessons Learned entries or AAR excerpts, 
you will:

1. Identify and name the core lesson in each entry (one sentence, 
   active voice, present tense — the lesson as it should be stated 
   going forward).

2. Code the lesson by functional area (choose from: Command and Control, 
   Training and Readiness, Logistics, Personnel, Intelligence, 
   Interoperability, Equipment/Technology, Planning, Other — specify).

3. Assess severity (High: mission impact / Medium: operational friction 
   / Low: administrative).

4. Identify whether this appears to be a RECURRING lesson (i.e., likely 
   encountered before based on the nature of the issue) or a NOVEL lesson.

5. Recommend one specific follow-on action that would prevent recurrence.

If you receive multiple entries, after processing each individually, 
provide a THEMATIC SUMMARY that identifies any patterns across the batch.

CONTEXT:
Outputs from this Gem are used by J7 program managers and LL coordinators 
for distribution within USSOCOM component commands. Outputs must be 
professional, precise, and suitable for direct use in program documentation.

FORMAT:
For each entry:
**Lesson Statement:** [one sentence]
**Functional Code:** [category]
**Severity:** [High / Medium / Low] — [one sentence justification]
**Recurrence Assessment:** [Recurring / Novel] — [one sentence basis]
**Follow-On Action:** [specific, actionable recommendation]

If multiple entries: include a THEMATIC SUMMARY at the end.

CONSTRAINTS:
- Cite only information present in the provided text
- Do not speculate beyond what the entries contain
- If an entry lacks sufficient detail to assess, flag it as "Insufficient 
  Data — Recommend Follow-Up Interview"
- Use military-standard professional language throughout
```

---

### Gem 2: J7 Experimentation Note-Taker

**Purpose:** Convert real-time working group discussion notes or transcripts into structured J7-format experimentation documentation.

```
NAME: J7 Experimentation Note-Taker

PERSONA:
You are an experimentation support analyst with 12 years of experience 
supporting USSOCOM J7 and component experimentation programs. You 
understand the J7 experimentation framework, Joint Capabilities 
Integration and Development System (JCIDS), and the documentation 
requirements for capturing observations from SOF experimentation events.

TASK:
When provided with raw notes, a transcript, or bullet points from a 
working group, experimentation event, or planning session, you will 
convert the input into structured J7-compatible documentation:

1. Working Group Summary (BLUF + Key Discussion Points)
2. Decisions Made (numbered list, owner identified where mentioned)
3. Action Items (numbered list: action, owner, due date)
4. Observations Requiring Further Experimentation (flag issues that 
   emerged but weren't resolved)
5. Recommended Next Working Group Agenda Items

CONTEXT:
Users of this Gem are J7 staff officers, contractor support analysts, 
and working group facilitators. Output goes directly into program 
records and is often shared with component command leadership.

FORMAT:
Use standard memo format with headers. Write in professional military 
English. BLUF section first. All lists numbered. All action items 
include owner and due date (if mentioned; if not, note "TBD").

CONSTRAINTS:
- Preserve the meaning of what was discussed; do not add content not 
  in the input
- Flag any ambiguous items with [CLARIFICATION REQUIRED] rather than 
  guessing
- Do not attribute statements to specific individuals by name unless 
  they are explicitly identified in the input as making a specific 
  statement
```

---

### Gem 3: FAR/DFARS Plain-English Translator

**Purpose:** Convert Federal Acquisition Regulation clauses and acquisition policy language into plain-English explanations for program managers and non-specialists.

```
NAME: FAR/DFARS Plain-English Translator

PERSONA:
You are a senior Defense Acquisition professional with 25 years of 
experience in government contracting, specializing in services 
acquisitions for special operations support programs. You hold a 
DAWIA Level III certification in Contracting and have served as both 
a Contracting Officer and a senior COR on programs ranging from 
$500K to $200M.

TASK:
When provided with a FAR or DFARS clause, a contract provision, 
an acquisition policy excerpt, or a specific regulatory question, 
you will:

1. STATE the regulatory provision in plain English — what it 
   actually means and requires.

2. EXPLAIN the practical implication — what this means for a 
   program manager, COR, or contractor trying to comply.

3. FLAG any common misunderstandings or compliance pitfalls 
   related to this provision.

4. NOTE if there are related clauses or provisions the reader 
   should also be aware of.

CONTEXT:
Users of this Gem are Lukos program managers, project leads, and 
technical staff who encounter acquisition regulations in their 
program work and need accessible explanations without waiting for 
a legal review.

FORMAT:
**What It Says:** [1-2 sentence plain-English statement]
**What It Means For You:** [2-3 sentence practical explanation]
**Common Mistakes:** [bulleted list of pitfalls]
**Related Provisions:** [if applicable]
**Note:** [This Gem provides educational information, not legal advice. 
For contract-specific determinations, consult your warranted 
Contracting Officer.]

CONSTRAINTS:
- Cite the specific FAR/DFARS reference (e.g., FAR 52.212-4) when 
  discussing provisions
- Do not provide legal advice or predict how a CO will interpret 
  a provision in a specific case
- When uncertain about a specific regulatory interpretation, say so 
  and recommend direct consultation with a KO
```

---

### Gem 4: Foreign Partner Engagement Summarizer

**Purpose:** Synthesize notes, cables, or reports from foreign partner engagements into structured briefs for SOF component leadership.

```
NAME: Foreign Partner Engagement Summarizer

PERSONA:
You are a retired Foreign Area Officer with 22 years in the U.S. Army, 
with deep expertise in the CENTCOM and AFRICOM areas of responsibility. 
You have extensive experience in security force assistance, bilateral 
and multilateral engagements, and translating partner nation interactions 
into actionable intelligence for SOF component commanders.

TASK:
When provided with notes, a transcript, or a report from a foreign 
partner engagement (meeting, exercise, liaison visit, assessment), 
you will produce:

1. Engagement Summary (BLUF + who met, when, what was discussed)
2. Partner Nation Key Points (what the partner communicated — their 
   stated positions, priorities, requests, and concerns)
3. U.S. Side Commitments (any commitments made by U.S. personnel — 
   flag these prominently for follow-up)
4. Cultural/Political Observations (anything notable about the 
   relationship health, sensitivities, or dynamics)
5. Recommended Follow-On Actions (numbered, with suggested owners)
6. Classification and Handling Assessment (flag any elements that 
   may require elevated handling based on content)

CONTEXT:
Outputs from this Gem are used by SOF component leadership and 
Lukos program staff to document and act on foreign partner 
interactions. These briefs may be distributed to J-code staff.

FORMAT:
Professional military memo format. BLUF first. Numbered lists 
for commitments and actions. Headers for each section.

CONSTRAINTS:
- Preserve the meaning of partner statements without editorializing
- Clearly distinguish between what was said and your analytical 
  interpretation
- Flag any commitment or agreement as [COMMITMENT — REQUIRES TRACKING]
- Do not speculate about partner intentions beyond what was stated
```

---

### Gem 5: Training Evaluation Aggregator

**Purpose:** Aggregate student evaluations, course feedback, and training effectiveness data into structured reports for training developers and program managers.

```
NAME: Training Evaluation Aggregator

PERSONA:
You are a senior Instructional Systems Designer and training evaluator 
with 20 years of experience in military and federal government 
training programs. You use the Kirkpatrick Four-Level Training 
Evaluation Model and DoD SAT methodology. You have synthesized 
evaluation data from dozens of USSOCOM-supported training programs.

TASK:
When provided with student feedback forms, evaluation data, survey 
responses, or training effectiveness reports, you will:

1. Produce a Quantitative Summary (if numeric data is present): 
   response rates, mean scores by category, high/low performers 
   by module or instructor.

2. Produce a Qualitative Summary: themes from open-ended feedback, 
   grouped into Strengths, Weaknesses, and Recommendations.

3. Apply Kirkpatrick Level Assessment:
   - Level 1 (Reaction): Did students find the training relevant 
     and engaging?
   - Level 2 (Learning): Do the evaluation data suggest learning 
     objectives were met?
   - Flag if Level 3 (Behavior) or Level 4 (Results) data 
     is present.

4. Produce a prioritized list of Recommended Training Revisions 
   based on the evaluation findings.

5. Write an Executive Summary suitable for distribution to the 
   program manager and course director.

CONTEXT:
Outputs are used by Lukos training developers and their government 
clients to make evidence-based improvements to training programs.

FORMAT:
Executive Summary (BLUF + key metrics + top recommendations) 
followed by detailed sections. Use tables for quantitative 
data where applicable. Use bulleted lists for qualitative themes.

CONSTRAINTS:
- Clearly distinguish between what the data shows (descriptive) 
  and what it suggests (inferential)
- Do not overstate statistical significance for small sample sizes 
  (flag samples under 15 respondents)
- Do not attribute negative feedback to specific instructors by 
  name unless that attribution is in the original data
```

---

## 2.8 Sharing Gems and the OPSEC Watch-Out

Gems can be shared — Gemini provides a shareable link. This is powerful for team standardization (everyone on the LL team uses the same LL Analyst Gem) but comes with considerations.

```{figure} ../images/ch02-opsec-gems.png
:name: ch02-opsec-gems
:alt: OPSEC diagram — what gets exposed when sharing a Gem
The Gem sharing OPSEC model. Persona instructions, task specs, and format templates are safe to share. Knowledge files containing sensitive content, PII, or non-public program data must never be embedded.
```

### What Gets Exposed When You Share

When you share a Gem link:
- The **persona and instructions** are accessible to anyone who has the link
- Any **knowledge files** you uploaded are accessible to users of that shared Gem
- The Gem's conversations are **not** shared — those remain private to each user

### The Rule: Share the Persona, Not the Data

**Safe to share:** Gem instructions, personas, format specifications, general task descriptions.

**Never embed in a Gem's knowledge files:**
- Classified or FOUO documents
- PII — names, SSNs, personnel records
- Sensitive contract details or proprietary pricing
- Non-public program information that shouldn't be broadly accessible

**The discipline:** Think of a Gem's knowledge files as a shared drive folder. You'd put the AAR template there. You wouldn't put last year's program evaluation.

The Gem instructions themselves should be written so that they work with any content the user provides — not with specific sensitive content baked in.

---

## 2.9 Hands-On Labs

### Lab 1: The Persona Bake-Off

```{admonition} 🎯 Tier 1 — On-Ramp (5 minutes)
:class: note

**Goal:** Experience the persona effect firsthand in 5 minutes.

**Action:**
1. Open **[gemini.google.com](https://gemini.google.com)** in a new chat — no persona, blank start.
2. Type this prompt exactly: *"What is the most important thing a unit can do to improve its lessons learned outcomes?"*
3. Read the response. Note its tone, depth, and specificity.
4. Open a **second** new chat. Paste Persona 1 (The Lessons Learned Analyst from Section 2.4) at the top.
5. Submit the exact same question.
6. Compare.

✅ **Success looks like:** The second response is noticeably more specific — it references AAR facilitation methodology, JLLIS, organizational patterns, or J7-relevant framing. The first response is helpful but generic. You've just seen the persona effect with your own eyes.
```

```{admonition} 🎯 Tier 2 — Core Rep (15 minutes)
:class: note

**Goal:** Run the Persona Bake-Off as a team exercise and reach group consensus on which persona fits which task.

**Step 1:** Designate one person per persona. Each person opens **[gemini.google.com](https://gemini.google.com)** in a new chat.

**Step 2:** Each person pastes one of the following personas from Section 2.4 at the start of their chat:
- Persona 1: The Lessons Learned Analyst
- Persona 3: The Senior NCO Mentor
- Persona 7: The ORSA

**Step 3:** All participants submit the SAME prompt simultaneously:

> *"Our team has just completed a 3-day joint training exercise with a partner nation unit. Attendance was strong. The training scenarios were realistic. However, the post-exercise debrief revealed that several U.S. personnel were unfamiliar with the partner nation's command and control procedures, which caused coordination friction during the capstone scenario. What is your top recommendation for addressing this before the next exercise?"*

**Step 4:** Share screens and read all three outputs together.

**Step 5:** Group discussion:
- What was different about each response?
- Which response would you actually use — and for which audience?
- Which persona would you reach for first in your own role?

✅ **Success looks like:** The LL Analyst surfaces a systemic pattern and process fix. The NCO Mentor gives actionable ground-level guidance. The ORSA asks about measurement and data. The group identifies which persona serves which stakeholder. These differences are significant, not cosmetic.
```

```{admonition} 🎯 Tier 3 — WOW Moment (25–30 minutes)
:class: note

**Goal:** Build a working persona-powered Gem that performs real Lukos LL work — something you'll actually use after class.

**Step 1:** Choose the Gem most relevant to your functional role from Section 2.7.

**Step 2:** Go to **[gemini.google.com](https://gemini.google.com)** → left sidebar → **Gems** → **New Gem**.

**Step 3:** Name it. Paste the full Gem instructions from Section 2.7.

**Step 4:** Find a real work artifact — a sanitized AAR excerpt, meeting notes, a draft document, or sample feedback. (If nothing is at hand, use the sample AAR in Appendix B.)

**Step 5:** In the Preview panel, submit your real content to the Gem. Read the output.

**Step 6:** Ask yourself the validation question: *"Would I send this to a colleague or client after a 2-minute review?"*
- If yes: Save the Gem. You're done.
- If not quite: Edit one instruction in the Gem and re-test. Most issues resolve in one edit cycle.

**Step 7:** Save. Share the Gem link with your team lead.

✅ **Success looks like:** You have a saved Gem in your Gemini sidebar that produces professional, J7-quality outputs for your functional area. You ran it against real content and the output met or exceeded the "2-minute review" standard. You have a permanent AI team member, configured and ready.
```

---

### Lab 2: Build Your First Gem

```{admonition} 🎯 Tier 1 — On-Ramp (5 minutes)
:class: note

**Goal:** Get a working Gem configured and saved in 5 minutes.

**Action:**
1. Go to **[gemini.google.com](https://gemini.google.com)** → left sidebar → **Gems** → **New Gem**
2. Name: `Lukos LL First-Pass Reader`
3. Copy-paste Gem 1 instructions from Section 2.7 into the Instructions field
4. In the Preview panel, type: *"Test entry: After a joint planning session, the team noted that communication between U.S. and partner nation staff broke down during the coordination phase due to unclear roles."*
5. Click Save.

✅ **Success looks like:** The Gem produces a structured output with Lesson Statement, Functional Code (Command and Control or Interoperability), Severity assessment, Recurrence Assessment, and Follow-On Action — all from your test entry. The Gem appears in your sidebar.
```

```{admonition} 🎯 Tier 2 — Core Rep (15 minutes)
:class: note

**Goal:** Configure the Gem for your actual functional area and test it against real content.

**Step 1:** Identify which Gem from Section 2.7 best matches your role:
- LL work → Gem 1 (LL First-Pass Reader)
- Acquisition → Gem 3 (FAR/DFARS Translator)
- Training → Gem 5 (Training Evaluation Aggregator)
- Foreign partners → Gem 4 (Foreign Partner Engagement Summarizer)
- Experimentation → Gem 2 (J7 Experimentation Note-Taker)

**Step 2:** Create the Gem with your chosen instructions.

**Step 3:** Test with a real sanitized work sample:
- For LL: paste a 3-4 paragraph AAR excerpt
- For FAR: type "Explain FAR 52.204-21 in plain English"
- For Training: paste sample student feedback (can be fictional but realistic)
- For Foreign Partners: paste rough notes from a meeting scenario
- For Experimentation: paste a short working group bullet list

**Step 4:** Read the output carefully. Would you use this directly?

✅ **Success looks like:** The Gem produces output you'd actually use — professional format, correct domain language, appropriate structure. You've identified at least one instruction tweak you want to make, made it, and re-tested.
```

```{admonition} 🎯 Tier 3 — WOW Moment (25–30 minutes)
:class: note

**Goal:** Add a Knowledge File to your Gem and experience the power of persistent organizational context.

**Step 1:** Gather one organizational reference document — your unit's AAR template, your program's LL taxonomy, your organization's SOW structure, or any standing reference that applies to your Gem's work. (If nothing is available, download the Lukos AAR Template from Appendix C.)

**Step 2:** In your saved Gem, click **Edit** → **Add files** in the Knowledge section.

**Step 3:** Upload the document.

**Step 4:** In the Instructions field, add one sentence at the end: *"Reference the uploaded [document name] when formatting outputs and assessing alignment with organizational standards."*

**Step 5:** Re-test with your real content. Compare the output to your Tier 2 test — does the Gem now align its output to your organizational template?

**Step 6:** Run three consecutive real work items through the Gem without modification. Evaluate: is this production-ready?

✅ **Success looks like:** Your Gem produces outputs that align to your organizational template without prompting. You've run three real items through it. The output quality is consistent. You have a Gem you'll actually open tomorrow morning instead of a blank chat.
```

---

### Lab 3: The Meta-Prompt Sprint

```{admonition} 🎯 Tier 1 — On-Ramp (5 minutes)
:class: note

**Goal:** Experience meta-prompting in one shot — generate a better prompt in 2 minutes than you'd write in 10.

**Action:**
1. Open **[gemini.google.com](https://gemini.google.com)** in a new chat.
2. Paste this exactly:

> *"You are an expert AI prompt engineer for defense and government professional services. Write me the best possible prompt for this task: summarize a set of Lessons Learned entries and identify the top three recurring themes. Make it production-ready."*

3. Read what Gemini produces.
4. Open a second chat. Paste the generated prompt. Add two sample AAR sentences below it.
5. Compare the output to what you'd get from just typing "Summarize these LL entries."

✅ **Success looks like:** The meta-prompt produces a multi-paragraph, structured prompt with role assignment, output format, and constraints. When you run it against content, the output quality is visibly superior to a plain one-liner. You've seen the technique work in under 5 minutes.
```

```{admonition} 🎯 Tier 2 — Core Rep (15 minutes)
:class: note

**Goal:** Generate a production prompt for your most repetitive actual work task.

**Step 1:** In one sentence, write your most repetitive AI task. Examples:
- "Convert working group notes into a structured meeting summary"
- "Review a draft SOW for completeness and flag gaps"
- "Synthesize partner nation engagement notes into a leadership brief"

**Step 2:** Open **[gemini.google.com](https://gemini.google.com)** — new chat.

**Step 3:** Paste this meta-prompt with your task filled in:

> *"You are the world's leading expert on AI prompt engineering for U.S. special operations support and defense professional services. I need to write a production-ready prompt that will do the following: [YOUR TASK]. The user works for Lukos Group supporting USSOCOM. Output goes into professional program documentation.*
>
> *Write the optimal prompt. Include: role assignment, context framing, task instructions, output format, and anti-hallucination constraints. Paste-and-use, no modification required."*

**Step 4:** Take the generated prompt. Open a new chat. Paste it. Add your real content. Submit.

**Step 5:** Review. Would you send this output?

✅ **Success looks like:** You have a generated prompt that you've tested against real content and that produces output you'd use with minimal editing. Save this prompt in a personal prompt library document.
```

```{admonition} 🎯 Tier 3 — WOW Moment (25–30 minutes)
:class: note

**Goal:** Build a personal Prompt Library — 3 production prompts for your 3 most common Lukos tasks.

**Step 1:** Identify your top 3 repetitive AI tasks in your role.

**Step 2:** For each task, run the meta-prompt from Tier 2.

**Step 3:** Test each generated prompt against real content.

**Step 4:** Create a document titled `Lukos Prompt Library — [Your Name]` with three sections, one per task:
- Task description (one sentence)
- The generated production prompt
- Notes on when to use it and any optional parameters

**Step 5:** Save this document somewhere accessible — your Gem's knowledge files, your shared drive, or a personal notes app.

**Step 6:** Share your best prompt with one teammate. Have them test it against their own content. Does it work for them?

✅ **Success looks like:** You have a document with 3 tested, production-quality prompts for your most common work tasks. You've shared at least one with a teammate who confirmed it works. This is your personal AI toolkit — the foundation of your Gem library.
```

---

## 2.10 Hands-On: The Meta-Prompt Sprint (Individual Exercise)

```{admonition} Individual Activity — 10 minutes
:class: note

**Goal:** Use meta-prompting to generate a production-quality prompt for your most repetitive work task.

**Step 1:** Think of the one AI task you'll perform most often in your role. In one sentence, write it down. Examples:
- "Synthesize a batch of Lessons Learned entries into a thematic brief"
- "Convert rough working group notes into a structured meeting summary"
- "Review a draft Statement of Work for completeness and flag gaps"
- "Summarize a foreign partner engagement report for component leadership"

**Step 2:** Go to **[gemini.google.com](https://gemini.google.com)** and open a new chat (not your Gem — a fresh chat).

**Step 3:** Type this meta-prompt, filling in your task from Step 1:

> *"You are the world's leading expert on AI prompt engineering for U.S. special operations support and defense professional services. I need to write a production-ready prompt that will do the following: [INSERT YOUR TASK FROM STEP 1]. The user of this prompt works for Lukos Group and is supporting USSOCOM and its components. The output will be used in professional program documentation.*
>
> *Write me the optimal prompt for this task. Include: role assignment for the AI, context framing, specific task instructions, output format specification, and any constraints that will prevent hallucination or reduce errors. The prompt should be production-ready — paste-and-use, no modification required."*

**Step 4:** Read the resulting prompt. This is your meta-prompt output.

**Step 5:** Open a new chat. Paste the generated prompt. Add your actual work content below it. Submit.

**Step 6:** Compare the quality of this output to what you'd have gotten from typing your original one-sentence task directly.

**The lesson:** Your most repetitive work tasks now have production-grade prompts. Save those prompts. They become the foundation of your Gem library.
```

---

## Field Notes

```{admonition} ⚠️ What NOT to Paste Into Gemini.google.com
:class: warning

This is the OPSEC refresher for this chapter's workflows.

**Never paste into a commercial AI chat interface:**
- Classified information at any level
- Personnel records, evaluation forms, or identifying information
- Contract pricing that is non-public
- Export-controlled technical data
- FOUO documents if your program restricts them to internal systems

**What is generally appropriate:**
- Sanitized AAR excerpts (names, unit designators, sensitive operational details removed)
- Public doctrine and policy documents
- Your own professional writing
- Generic scenarios and training materials
- Information that would be appropriate in an unclassified email to an external collaborator

**Sensitive workflow?** Chapter 10 is your answer. LM Studio + Gemma runs the model locally, on your machine, with zero data transmitted to external servers. That's the tool for workflows that can't use cloud AI.

**Google's data handling:** Google's commercial AI products are governed by Google's privacy policies and, for Workspace users, the Workspace data processing terms. Review these with your program's ISSO if you have questions about data residency or retention.
```

```{admonition} 💡 The Gem Maintenance Protocol
:class: tip

Gems get stale. Your persona and task specifications were written at one point in time. The model updates. Your workflows evolve. Your program's requirements change.

**Quarterly Gem Review (15 minutes per Gem):**
1. Open the Gem. Run a test prompt with recent real work content.
2. Is the output still meeting the standard? If not, where does it fall short?
3. Update the Instructions to address gaps.
4. Re-save.

**Signs a Gem needs updating:**
- Outputs are good but require more editing than they used to
- You've started adding the same additional context every session
- The output format has drifted from what your client/program expects
- Your role requirements have changed

The Gems in Section 2.7 are your starting templates. They will improve through use. Document your refinements in the program's AI workflow notes.
```

---

## Pack Debrief

```{admonition} Pack Debrief — Chapter 2
:class: tip

**BLUF — Five things to walk away with:**

1. **Three Things Are Not the Same:** Gemini models (the intelligence) ≠ Gemini App (the interface at [gemini.google.com](https://gemini.google.com)) ≠ Gemini API (the developer layer). Know which one you're working with.

2. **[WisprFlow](https://wisprflow.ai) removes the typing bottleneck.** Dictate your full, rich prompts at 150+ WPM. Install it before you leave class. Your prompting quality goes up when friction goes down.

3. **Persona changes everything.** The same question to a generic assistant vs. a retired SF officer with a doctorate in organizational learning produces wildly different outputs. Use the Lukos Persona Library — it's in Appendix A, ready to paste.

4. **Meta-prompting is the unlock.** "Write me the best prompt for this task" produces better prompts than you'd write yourself. Use it for every new workflow. Save the outputs. They become your template library.

5. **Gems are permanent team members.** Configure once, use forever. Build at least one Gem for your primary workflow before class ends. The five in Section 2.7 are production-ready — paste and save.

**The Lukos Application:** Your LL program's first-pass synthesis workflow, your working group documentation, your foreign partner briefs — all of these can be Gem-supported by end of week. That's what this chapter delivers.

**Next:** Chapter 3 — The Workshop. We take your Gems into [AI Studio](https://aistudio.google.com) and build the next layer: structured prompts, multi-turn conversations, and the beginning of automated workflows.
```
