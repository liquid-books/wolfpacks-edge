# Lukos: High-Value Tutorials and Use Cases

*Practical, copy-paste guides for getting the most out of your AI stack — tested on real Lukos workflows.*

---

## Tutorial 1: Adding 8 Free MCP Servers to Google Antigravity

A practical, copy-paste tutorial for getting useful MCP (Model Context Protocol) servers running inside Google Antigravity with the absolute minimum setup.

---

## "But these say Anthropic — will they work with Gemini?"

**Yes. All of them.**

This is the single most common confusion about MCP. MCP is an **open protocol** that any AI tool can speak — Claude, Gemini, GPT, local models, all of them. Anthropic created the protocol and wrote some reference servers (Filesystem, Memory, Sequential Thinking), but the servers themselves contain no Claude-specific code. They expose tools via the MCP standard, and Gemini in Antigravity calls those tools the same way Claude would.

Google's own Antigravity MCP store even pre-lists Sequential Thinking, and Google's docs explicitly recommend the `@modelcontextprotocol/*` packages. The same applies to every other server in this guide — Upstash's Context7, Brave's, Firecrawl's, Google's own Chrome DevTools server. They're all just MCP servers. The model behind your IDE doesn't matter.

---

## What you'll install

**Tier 1 — zero-config, no API key required**

1. **Filesystem** — let the agent read/write your local files
2. **Memory** — persistent knowledge graph across sessions
3. **Sequential Thinking** — structured step-by-step reasoning
4. **DuckDuckGo Search** — web search with no signup

**Tier 2 — free, but needs a free API key or Chrome installed**

5. **Context7** — live, version-specific library documentation
6. **Brave Search** — higher-quality web search (2,000 queries/mo free)
7. **Firecrawl** — production-grade web scraping & content extraction (free tier)
8. **Chrome DevTools** — let the agent drive a real Chrome browser

---

## Prerequisites

Seven of the eight servers run via `npx`, so the only software you need is **Node.js**. The eighth (Chrome DevTools) also needs a recent **Google Chrome** installed.

### macOS

1. Install Node.js (LTS, version 20 or higher recommended).
   - Easiest path: download the macOS installer from <https://nodejs.org>
   - Or via Homebrew: `brew install node`
2. Verify in Terminal:
   ```bash
   node --version   # should print v20.x or higher
   npx --version
   ```
3. Make sure Google Chrome is installed (only for the Chrome DevTools server).

### Windows

1. Install Node.js (LTS, 20+) from <https://nodejs.org> using the official installer (NOT a third-party installer — Brave's docs note that some Windows users hit MCP issues with non-official installers).
2. Verify in PowerShell or Command Prompt:
   ```powershell
   node --version
   npx --version
   ```
3. On Windows, every `npx` command needs to be wrapped in `cmd /c` inside the JSON (examples below show both Mac and Windows variants).
4. Make sure Google Chrome is installed (only for the Chrome DevTools server).

**Python is NOT required** for any server in this tutorial. Everything uses the Node.js toolchain.

---

## How to add MCP servers in Antigravity

There are two ways. Pick whichever feels easier.

### Method A — Edit the raw config (recommended, takes 30 seconds)

1. Open Antigravity.
2. In the Agent panel on the right, click the **⋯** (three-dot) menu at the top.
3. Click **MCP Servers**. The MCP Store opens.
4. Click **Manage MCP Servers** at the top of the store.
5. In the main tab, click **View raw config**. The `mcp_config.json` file opens.
6. Paste the JSON snippets from this guide inside the `mcpServers` object.
7. Save. Antigravity will pick up the changes automatically (or click **Refresh** in the Manage MCP Servers tab).

### Method B — Ask Antigravity's agent to do it

In the Agent panel, paste a prompt like:

> Open my MCP config (Manage MCP Servers → View raw config) and add the Filesystem MCP server with `npx -y @modelcontextprotocol/server-filesystem` pointed at `/Users/myname/Projects`. Show me the diff before saving.

The agent can edit the file for you, but you still have to click into **Manage MCP Servers** once so the path is in scope. Method A is faster for a first install.

---

## 1. Filesystem MCP

**What it is:** Anthropic's official server that gives the agent sandboxed read/write access to specific local directories you whitelist.

**Why it's valuable:** Most "useful agent" workflows need files. Without this, your agent can only see the file you have open. With it, the agent can scan a project, read multiple files, write new ones, and search across folders.

**Use cases**

- "Read every Markdown file in my course folder and summarize the chapters."
- "Create a new file `recipe-67.md` and write it in the same style as the existing recipes."
- Bulk renames, refactors, and content audits across a directory.

**Prerequisites:** Node.js. No account, no key.

**Source:** <https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem>

### Install

Pick one or more directories you want the agent to access. **Whatever you whitelist, the agent can read AND write to.** Don't whitelist your whole home directory.

**macOS / Linux config:**

```json
"filesystem": {
  "command": "npx",
  "args": [
    "-y",
    "@modelcontextprotocol/server-filesystem",
    "/Users/yourname/Projects",
    "/Users/yourname/Documents/Notes"
  ]
}
```

**Windows config:**

```json
"filesystem": {
  "command": "cmd",
  "args": [
    "/c", "npx", "-y",
    "@modelcontextprotocol/server-filesystem",
    "C:\\Users\\YourName\\Projects",
    "C:\\Users\\YourName\\Documents\\Notes"
  ]
}
```

### Antigravity prompt to test it

> List every file in my Projects folder and tell me which ones haven't been modified in the last 30 days.

---

## 2. Memory MCP

**What it is:** Anthropic's official server that maintains a persistent knowledge graph (entities + relationships) on disk, so the agent can remember facts across chat sessions.

**Why it's valuable:** Antigravity sessions don't share state. Without Memory, you re-explain your project, your stack, and your preferences every time. With it, you tell the agent "remember that I'm using Supabase for auth" once and it sticks.

**Use cases**

- Long-running projects where context grows over weeks.
- Personal preferences ("I always use TypeScript, never use Tailwind, prefer functional components").
- Tracking decisions ("we decided NOT to use Kafka because of operational overhead").

**Prerequisites:** Node.js. No account, no key.

**Source:** <https://github.com/modelcontextprotocol/servers/tree/main/src/memory>

### Install

**macOS / Linux config:**

```json
"memory": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-memory"]
}
```

**Windows config:**

```json
"memory": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-memory"]
}
```

### Antigravity prompt to test it

> Remember these facts about me: I teach at FAU, I run MFunding, my preferred stack is Next.js + Supabase, and I keto. Now confirm what you've stored.

In the next session, ask: *"What do you remember about my teaching role?"*

---

## 3. Sequential Thinking MCP

**What it is:** Anthropic's official server that exposes a structured reasoning scratchpad. The agent uses it to break down problems into explicit, revisable thought steps before answering.

**Why it's valuable:** It's the single most token-cheap quality boost you can give an agent. Connects to no external service — it's pure structured prompting infrastructure.

**Use cases**

- Multi-step debugging ("why is this only failing in production?")
- Architecture comparisons ("compare three approaches and revise if assumption X breaks")
- Migration planning, risk analysis, anything where the model tends to rush.

**Prerequisites:** Node.js. No account, no key.

**Source:** <https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking>

### Install

**macOS / Linux config:**

```json
"sequential-thinking": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
}
```

**Windows config:**

```json
"sequential-thinking": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-sequential-thinking"]
}
```

### Antigravity prompt to test it

> Use sequential thinking to plan a migration of my MFunding lead database from Airtable to Supabase. Identify risks at each step and revise the plan if downtime would exceed 1 hour.

---

## 4. DuckDuckGo Search MCP

**What it is:** A free web search server using DuckDuckGo. Multiple flavors exist; this guide uses the `@oevortex/ddg_search` package because it works out of the box with no API key.

**Why it's valuable:** Antigravity's agent can't browse the web on its own. Adding DuckDuckGo gives it a free, no-signup search tool.

**Use cases**

- Quick fact-checking during a coding session.
- "Find me the latest npm package for X."
- Research without using your Brave Search API quota.

**Prerequisites:** Node.js. No account, no key.

**Source:** <https://github.com/OEvortex/ddg_search>

### Install

**macOS / Linux config:**

```json
"ddg-search": {
  "command": "npx",
  "args": ["-y", "@oevortex/ddg_search@latest"]
}
```

**Windows config:**

```json
"ddg-search": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "@oevortex/ddg_search@latest"]
}
```

### Antigravity prompt to test it

> Search the web for "Gemini 3 Pro release notes" and summarize the top 3 results.

---

## 5. Context7 MCP

**What it is:** A documentation server by Upstash that fetches **up-to-date, version-specific** docs for thousands of libraries and injects them into your prompt. Solves the "the model is using last year's API" problem.

**Why it's valuable:** When you're coding against any framework (Next.js, Supabase, FastAPI, GoHighLevel SDK, etc.), the model's training data is months or years stale. Context7 pulls the actual current docs.

**Use cases**

- "How do I set up Next.js 15 middleware? **use context7**"
- "Implement Supabase auth with email/password. **use library /supabase/supabase**"
- Any library work where you keep getting hallucinated APIs.

**Prerequisites:** Node.js. **API key is optional** — works without one at lower rate limits. Free key at <https://context7.com/dashboard> for higher limits.

**Source:** <https://github.com/upstash/context7>

### Install (remote — simplest, recommended)

This uses Context7's hosted server. No local process at all.

```json
"context7": {
  "url": "https://mcp.context7.com/mcp",
  "headers": {
    "CONTEXT7_API_KEY": "YOUR_API_KEY_HERE"
  }
}
```

If you skip the API key entirely, just remove the `headers` block — it'll work on the anonymous tier.

### Install (local fallback, if remote ever fails)

**macOS / Linux:**

```json
"context7": {
  "command": "npx",
  "args": ["-y", "@upstash/context7-mcp", "--api-key", "YOUR_API_KEY"]
}
```

**Windows:**

```json
"context7": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "@upstash/context7-mcp", "--api-key", "YOUR_API_KEY"]
}
```

### Antigravity prompt to test it

> Show me how to implement server-side streaming with the latest Vercel AI SDK. use context7

---

## 6. Brave Search MCP

**What it is:** Brave's official MCP server backed by their independent search index. Generally higher-quality results than DuckDuckGo for technical queries.

**Why it's valuable:** Better signal-to-noise on dev queries, cleaner snippets, and Brave's API includes news, image, video, and AI summary endpoints. Many devs prefer it to Claude Code's built-in web search.

**Use cases**

- Researching current best practices (e.g., "Next.js App Router auth 2026").
- News and trend queries that DuckDuckGo handles weakly.
- AI-powered summarization of search results in one call.

**Prerequisites:** Node.js + a free Brave Search API key.

### Get the API key (free, ~2 minutes)

1. Go to <https://api.search.brave.com> and sign up.
2. In the dashboard sidebar, go to **Subscriptions**, choose the **Free** plan (2,000 queries/month).
3. Go to **API keys**, click **Add API Key**, and copy it.

**Source:** <https://github.com/brave/brave-search-mcp-server>

### Install

**macOS / Linux config:**

```json
"brave-search": {
  "command": "npx",
  "args": ["-y", "@brave/brave-search-mcp-server", "--transport", "stdio"],
  "env": {
    "BRAVE_API_KEY": "YOUR_BRAVE_API_KEY_HERE"
  }
}
```

**Windows config:**

```json
"brave-search": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "@brave/brave-search-mcp-server", "--transport", "stdio"],
  "env": {
    "BRAVE_API_KEY": "YOUR_BRAVE_API_KEY_HERE"
  }
}
```

### Antigravity prompt to test it

> Use Brave Search to find the three most recent articles on MCP server security best practices, and give me an AI summary.

---

## 7. Firecrawl MCP

**What it is:** Firecrawl's official MCP server. Where Brave/DuckDuckGo *find* URLs, Firecrawl *extracts the content* of those URLs as clean, LLM-ready Markdown or structured JSON — even from sites with heavy JavaScript, anti-bot protection, or weird layouts. It also does deep crawling, batch scraping, sitemapping, and structured data extraction.

**Why it's valuable:** Search results give you snippets; Firecrawl gives you the actual page content. Pair it with Brave Search and you get a complete research stack: Brave finds the right pages, Firecrawl pulls them down clean. It also handles JavaScript-rendered sites that simple HTTP fetchers can't.

**Use cases**

- Pull real estate listings from Redfin and structure them into JSON for your CRM.
- "Scrape this competitor's pricing page and tell me what changed since last month."
- Research workflows: "Search for the top 5 articles on X, then scrape and synthesize them."
- Building your own RAG pipeline — Firecrawl outputs Markdown that's already chunked-friendly.
- Extract structured data from many URLs at once with a schema you define.

**Prerequisites:** Node.js + a free Firecrawl API key. Free tier gives you 500 credits to start. <https://firecrawl.dev/app/api-keys>

**Source:** <https://github.com/firecrawl/firecrawl-mcp-server>

### Get the API key (~1 minute)

1. Go to <https://firecrawl.dev> and sign up (free).
2. From the dashboard, go to **API Keys** and copy your key (it starts with `fc-`).

### Install — Option A: Remote URL (simplest, no local process)

Firecrawl hosts the server itself. Your API key goes right in the URL.

```json
"firecrawl": {
  "url": "https://mcp.firecrawl.dev/YOUR_FIRECRAWL_API_KEY/v2/mcp"
}
```

Replace `YOUR_FIRECRAWL_API_KEY` (including the `fc-` prefix) directly in the URL. Same config works on Mac and Windows.

### Install — Option B: Local via npx

**macOS / Linux:**

```json
"firecrawl": {
  "command": "npx",
  "args": ["-y", "firecrawl-mcp"],
  "env": {
    "FIRECRAWL_API_KEY": "fc-YOUR_API_KEY_HERE"
  }
}
```

**Windows:**

```json
"firecrawl": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "firecrawl-mcp"],
  "env": {
    "FIRECRAWL_API_KEY": "fc-YOUR_API_KEY_HERE"
  }
}
```

### Antigravity prompt to test it

> Use Firecrawl to scrape https://news.ycombinator.com and give me the top 5 stories with titles, links, and point counts as a Markdown table.

For a combo prompt that uses Brave + Firecrawl together:

> Use Brave Search to find the three most-cited 2026 articles on MCP server security. Then use Firecrawl to pull the full content of each, and write a 300-word synthesis comparing their recommendations.

---

## 8. Chrome DevTools MCP

**What it is:** Google's official MCP server that lets the agent drive a real Chrome browser via Chrome DevTools Protocol. The agent can navigate pages, click, fill forms, take screenshots, run performance traces, and inspect network/console.

**Why it's valuable:** Gives your coding agent eyes. It can actually verify the change it just shipped works in a browser, not just guess.

**Use cases**

- "Open localhost:3000, click 'Sign Up', fill the form, screenshot the result."
- "Run a performance trace on web.dev and tell me the LCP."
- Automated end-to-end testing prompted in natural language.
- Debugging "it works on my machine" — the agent can check.

**Prerequisites:**

- Node.js (recent versions of this server require Node 20+; if Antigravity errors on startup, upgrade Node).
- Google Chrome installed (any modern version).
- Chrome does NOT need to be running before you start; the server launches it on first tool call.

**Source:** <https://github.com/ChromeDevTools/chrome-devtools-mcp>

### Install

**macOS / Linux config:**

```json
"chrome-devtools": {
  "command": "npx",
  "args": ["chrome-devtools-mcp@latest"]
}
```

**Windows config:**

```json
"chrome-devtools": {
  "command": "cmd",
  "args": ["/c", "npx", "-y", "chrome-devtools-mcp@latest"]
}
```

### Antigravity prompt to test it

> Open https://web.dev in Chrome via Chrome DevTools, run a performance trace, and tell me the Largest Contentful Paint score.

---

## All-in-one config (copy-paste this and edit values)

This is the complete `mcp_config.json` with every server above. Replace the placeholders in CAPITALS.

### macOS / Linux

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/yourname/Projects"
      ]
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    },
    "ddg-search": {
      "command": "npx",
      "args": ["-y", "@oevortex/ddg_search@latest"]
    },
    "context7": {
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "YOUR_CONTEXT7_KEY_OR_REMOVE_THIS_BLOCK"
      }
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@brave/brave-search-mcp-server", "--transport", "stdio"],
      "env": {
        "BRAVE_API_KEY": "YOUR_BRAVE_API_KEY"
      }
    },
    "firecrawl": {
      "url": "https://mcp.firecrawl.dev/YOUR_FIRECRAWL_API_KEY/v2/mcp"
    },
    "chrome-devtools": {
      "command": "npx",
      "args": ["chrome-devtools-mcp@latest"]
    }
  }
}
```

### Windows

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "cmd",
      "args": [
        "/c", "npx", "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\YourName\\Projects"
      ]
    },
    "memory": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-memory"]
    },
    "sequential-thinking": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-sequential-thinking"]
    },
    "ddg-search": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@oevortex/ddg_search@latest"]
    },
    "context7": {
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "YOUR_CONTEXT7_KEY_OR_REMOVE_THIS_BLOCK"
      }
    },
    "brave-search": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@brave/brave-search-mcp-server", "--transport", "stdio"],
      "env": {
        "BRAVE_API_KEY": "YOUR_BRAVE_API_KEY"
      }
    },
    "firecrawl": {
      "url": "https://mcp.firecrawl.dev/YOUR_FIRECRAWL_API_KEY/v2/mcp"
    },
    "chrome-devtools": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

---

## Verification

After saving the config:

1. In Antigravity, open **Manage MCP Servers** and click **Refresh**.
2. Each server should show a green status. The first time, `npx` will download the package — that can take 10–30 seconds per server.
3. Try the test prompt under each section above.

---

## Troubleshooting

- **Server stays grey or red:** Open Antigravity's Output/Logs pane and look for the actual error. 9 times out of 10 it's a typo in JSON or a wrong path in Filesystem's whitelist.
- **`npx` not found:** Node isn't on the PATH that Antigravity is using. Reinstall Node from the official installer and restart Antigravity completely (not just reload).
- **Windows: server hangs forever:** You forgot the `cmd /c` wrapper. Every Windows entry needs `"command": "cmd"` and `/c` as the first arg.
- **Chrome DevTools fails:** Update Node to v20+. If on macOS and you see a permissions error, grant Antigravity Bluetooth/Automation permissions in **System Settings → Privacy & Security**.
- **Brave Search returns 401:** API key is wrong, or you didn't subscribe to a plan in the Brave dashboard. Subscribing to the Free plan is required even though it's free.
- **Firecrawl returns 401 or "credits exhausted":** Check the API key in your URL/env (must include the `fc-` prefix). Free tier is limited — check usage at <https://firecrawl.dev/app>.
- **Context7 says rate-limited:** Get a free API key at <https://context7.com/dashboard>.
- **Too many tools slowing things down:** Each server's tool definitions consume context tokens (roughly 500–1,000 per tool). If you have all 8 active, you're easily spending 10k+ tokens before the agent does anything. Disable servers you aren't actively using by deleting their block from the config.

---

## What to add next

Once these are humming, the natural next picks (also free, slightly more setup) are:

- **GitHub MCP** (remote, OAuth) — open PRs, triage issues, search code.
- **Playwright MCP** — heavier browser automation than Chrome DevTools.
- **Notion / Figma** — if you live in either, the OAuth setup is one click.

---

*Last updated April 2026.*

---

## Tutorial 2: Lukos LLC — 7 Federal Contracting Workflows for Antigravity

A step-by-step, prompt-driven tutorial for non-programmers. You'll start by running a one-time **Setup skill** that creates your project structure and your `GEMINI.md` system prompt. Then you'll build seven reusable skills for the work Lukos actually does every day.

---

### Before you start

You should already have these MCP servers installed in Antigravity from Tutorial 1:

- **Firecrawl** (web scraping and API calls)
- **Brave Search** (better web search)
- **Memory** (persistent knowledge across sessions)
- **Sequential Thinking** (structured reasoning)

You do **not** need the Filesystem MCP. Antigravity already has full file access inside your workspace — adding the Filesystem MCP just creates noise and risk without adding capability.

DuckDuckGo, Context7, and Chrome DevTools are nice to have but not required for the seven use cases here.

---

### Free vs. Paid: what you get from Antigravity

Antigravity is free to start using — no credit card needed. But like most AI platforms, there are limits, and they've shifted since launch. Here's the picture as of April 2026 (always check <https://antigravity.google/pricing> for the latest, since Google has adjusted these multiple times):

**Free tier — $0/month**
- Access to Gemini 3 Pro, Claude Sonnet 4.6, Claude Opus 4.6, and other supported models
- Unlimited tab completions (autocomplete-style suggestions while you type)
- A weekly quota of "agent requests" (the more substantial work — the kind we'll be doing in this tutorial)
- Quotas have been adjusted several times. At launch the free tier got 250 agent requests per day; that's been cut to roughly 20/day, now packaged as a larger weekly bucket
- Good enough to evaluate the platform and run light Lukos workflows (a few opportunity hunts and meeting briefs per week)

**AI Pro — $20/month**
- Higher rate limits, with quota refreshing every 5 hours instead of weekly
- Built-in credits for heavier work
- Same model access as the free tier
- This is the practical sweet spot for one person doing daily Lukos work

**AI Ultra — $249.99/month**
- The highest rate limits and biggest credit allocation
- Aimed at heavy daily use of the most expensive models (Claude Opus 4.6 in particular)
- Probably overkill unless multiple Lukos staff are running agentic workflows all day

**Pay-as-you-go credits**
- $25 for 2,500 credits, or $199 for 20,000
- Used when you exhaust your plan's quota
- Google hasn't published exactly how many tokens one credit buys — if you go this route, monitor usage carefully

**A few honest caveats**
- "Usage is correlated with work done" — a complex multi-step task (scraping SAM.gov + scoring + saving a digest) eats more quota than a quick question. Plan accordingly.
- Some Pro users have reported unexpected lockouts when they hit weekly limits. Have a backup plan for critical work (e.g., do the most important opportunity hunts first thing on quota refresh day).
- Free tier is enough to **learn** this tutorial and prove the seven workflows are valuable. If you decide it's worth keeping, Pro at $20/month is the obvious next step.

**What this means for Lukos:** start on the free tier, run through Use Cases 0–3 to see if the workflows fit your team. If they do, upgrade one staff member to Pro and let them run the daily/weekly skills. You'll know within two weeks whether it's worth the $20.

---

### How this tutorial works

Each use case follows the same structure:

1. **What it does** — the business outcome in one paragraph.
2. **Tools needed** — split into **Mandatory** and **Optional**. Fewer tools = fewer things that can break. Start with mandatory only.
3. **Path A — No account, no API key.** The fastest way to get something working.
4. **Path B — Free account + free API key.** A more reliable, production-ready version once you've proven the workflow.
5. **Try it now** — exact prompts to paste into Antigravity's Agent panel.
6. **The skill** — a copy-paste skill file with proper YAML frontmatter you can save as `.agents/skills/[name]/SKILL.md`.

:::{note}
**What is a skill?** A skill is a saved instruction file that tells the AI agent: "When the user asks for X, follow these steps using these tools." Save each one as `.agents/skills/[name]/SKILL.md` in your project. Antigravity will automatically pick up the right skill when you describe what you want — no slash commands required.
:::

---

## Use Case 0: Set Up Your Lukos Workspace (do this first, once)

This is a one-time setup. About 5 minutes. You'll create a folder on your computer, open it in Antigravity, drop in a special file called `GEMINI.md` that tells the AI who Lukos is, then test that it actually worked.

You don't need any MCP tools or accounts for this — just your computer and Antigravity.

### Step 1: Create the workspace folder on your computer

#### On Mac

1. Open **Finder**.
2. In the sidebar on the left, click **Downloads**.
3. Right-click in the empty space and choose **New Folder**.
4. Name it `Lukos-Workspace` (use the dash, no spaces).
5. Press Return.

#### On Windows

1. Open **File Explorer** (the yellow folder icon in the taskbar).
2. In the sidebar on the left, click **Downloads**.
3. Right-click in the empty space and choose **New** → **Folder**.
4. Name it `Lukos-Workspace` (use the dash, no spaces).
5. Press Enter.

That's your project folder. Everything for Lukos work will live here.

### Step 2: Open the folder in Antigravity

1. Open **Antigravity**.
2. Click **File** in the top menu, then **Open Folder…**
3. Navigate to your **Downloads** folder.
4. Click once on `Lukos-Workspace` to select it, then click **Open**.
5. If Antigravity asks "Do you trust the authors of this folder?", click **Yes, I trust the authors**. This is your own folder, so it's safe.

You should now see `LUKOS-WORKSPACE` in the file explorer panel on the left. It's empty — that's correct.

### Step 3: Create GEMINI.md (the company context file)

`GEMINI.md` is a special file Antigravity reads automatically every time you start a chat. It's where Lukos's company info lives so you don't have to re-explain things every session.

1. Open the **Agent panel** on the right side of Antigravity (look for the chat icon).
2. Copy the entire prompt below and paste it into the chat box, then send.

> Create a file named `GEMINI.md` in the root of this workspace. Fill it with exactly the following content, then confirm when done:
>
> ```markdown
> # Lukos LLC — Agent Operating Context
>
> ## About this organization
> Lukos LLC is a Tampa, FL-based federal services contractor. We are a Service-Disabled Veteran-Owned Small Business (SDVOSB) and certified 8(a). Founded 2008 by military veterans. Our culture is "the Wolfpack" — we work as a team, we hire veterans first, and we communicate as peers, not as corporate recruiters.
>
> ## Our core capabilities
> - Training and operations support
> - Healthcare advice and medical training
> - Behavioral health services
> - Acquisition program support (FAR/DFARS expertise)
> - Systems engineering
> - R&D support
> - Logistics planning
> - Maximizing human performance
>
> ## Our customers
> Primary: USSOCOM, SOCCENT, NSW (Naval Special Warfare), other Special Operations forces.
> Federal civilian: CDC (Epidemiology contracts), DHS ICE.
> We support major joint commands and acquisition program offices across all five services.
>
> ## NAICS codes we target
> - 541330 — Engineering Services
> - 541611 — Admin Management & General Management Consulting
> - 541612 — HR Consulting Services
> - 541618 — Other Management Consulting
> - 541690 — Other Scientific & Technical Consulting Services
> - 541715 — R&D in Physical, Engineering, and Life Sciences
> - 611430 — Professional and Management Development Training
> - 621112 — Offices of Physicians, Mental Health Specialists
>
> ## Set-asides we are eligible for
> - SDVOSBC (Service-Disabled Veteran-Owned Small Business Set-Aside)
> - 8AC (8(a) Set-Aside)
> - Total Small Business Set-Aside
>
> ## Voice and tone
> - Peer-to-peer, not corporate-to-recruit
> - Warm but professional
> - Reference military service when relevant — we speak the same language
> - Use "Wolfpack" culture references in recruiting outreach
> - Never use generic recruiter clichés ("rockstar," "ninja," "synergy")
> - Plain English, not jargon, when explaining contracts internally
>
> ## Compliance defaults
> - Always cite FAR/DFARS clauses by their official number
> - Verify clause text against acquisition.gov before summarizing — do not rely on training data
> - Never invent past performance details — if data isn't in our knowledge base, ask before answering
> - For dollar values and dates, prefer official sources (SAM.gov, USAspending.gov) over secondary reporting
>
> ## Data handling
> - Use only public sources for candidate research
> - Never log in to LinkedIn or other gated platforms
> - Reference customer details only when explicitly stored in our past performance knowledge base
> ```

The agent will create the file. After a few seconds you should see `GEMINI.md` appear in the file explorer panel on the left.

### Step 4: Test that it actually works

**First, fully close Antigravity and reopen it.** Then open the `Lukos-Workspace` folder again. Closing and reopening forces a fresh load of GEMINI.md.

Open the Agent panel and paste this exact prompt:

> What is our company culture called?

**If GEMINI.md is working correctly, the agent will answer: "Wolfpack."**

If the agent says it doesn't know, or asks "who is 'we'?", check:
- The file is named exactly `GEMINI.md` (all caps, with the dot)
- It's in the workspace root, not inside a subfolder
- Try closing and reopening Antigravity once more

For one more confidence check:

> What NAICS codes do we target?

The agent should list 541330, 541611, 541612, 541618, 541690, 541715, 611430, and 621112. Once both tests pass, you're done. Every chat in this workspace now starts already knowing who Lukos is — no more re-explaining the company every session.

### Optional: Save this setup as a reusable skill

In your `Lukos-Workspace`, ask the agent:

> Create a folder called `.agents/skills/lukos-project-setup/` and inside it create a file called `SKILL.md` with the content I'm about to give you:

```markdown
---
name: lukos-project-setup
description: Bootstrap a new Antigravity project for Lukos federal contracting work. Creates the standard project folder structure and writes GEMINI.md with Lukos's company context (NAICS codes, target agencies, voice, compliance defaults). Run once at the start of any new Lukos project.
---

# Skill: Lukos Project Setup

## Triggers
Use this skill when the user says: "set up this project for Lukos,"
"initialize a Lukos workspace," or "bootstrap Lukos workflows."

## Required tools
None. Use Antigravity's built-in file creation.

## Workflow
1. Confirm the workspace path with the user before creating files.
2. Create the following folders in the workspace root if they
   don't already exist:
   - `digests/` — for daily SAM.gov opportunity output
   - `rfps/active/` — for incoming RFP PDFs to process
   - `rfps/archive/` — for completed/abandoned RFPs
   - `briefs/` — for pre-meeting intelligence briefs
   - `candidates/` — for sourcing output and outreach drafts
   - `.agents/skills/` — where future skill files live
3. Create `GEMINI.md` in the workspace root with Lukos's standard
   company context (see Use Case 0 of the Lukos tutorial for the
   exact content — copy it verbatim).
4. Confirm completion with a summary listing every folder and file
   that was created.

## Output
A short confirmation:
"Lukos workspace initialized. Created:
- digests/ (daily opportunity output)
- rfps/active/ and rfps/archive/ (RFP processing)
- briefs/ (meeting intelligence briefs)
- candidates/ (sourcing output)
- .agents/skills/ (future skill files)
- GEMINI.md (Lukos context — auto-loaded every session)

Test it by asking 'What is our company culture called?' — the answer should be 'Wolfpack.'"
```

You're now ready for the rest of the use cases.

---

## Use Case 1: Daily SAM.gov Opportunity Hunter

### What it does

Every morning, the agent scans SAM.gov for newly posted federal contract opportunities matching Lukos's criteria. It produces a ranked digest with a "should we bid?" score for each one and saves it to the `digests/` folder you can review with coffee.

### Tools needed

- **Mandatory:** Firecrawl
- **Optional:** Memory (so the agent remembers which opps you've already seen), Sequential Thinking (better scoring logic)

### Path A — No account, no API key (the manual approach)

SAM.gov's public search page is heavily JavaScript-driven, and you can't construct a working filter URL by hand. The good news: you can apply your filters in the UI once, copy the resulting URL from your browser, and reuse it forever.

**Building your search URL:**

1. Open <https://sam.gov/opportunities> in your browser.
2. In the search panel on the left, apply your filters:
   - **Notice Type** → check Solicitation, Combined Synopsis/Solicitation, Presolicitation
   - **Set Aside** → check the ones Lukos qualifies for (SDVOSB, 8(a), Total Small Business)
   - **NAICS Code** → enter your target codes one at a time (e.g., 541330)
   - **Date** → set the Posted Date range (e.g., last 7 days)
3. Click **Search**. You'll see results.
4. **Copy the URL from your browser's address bar.** Save it somewhere.

**Try it now**

> Use Firecrawl to scrape this SAM.gov URL: `YOUR_SAM_URL`
>
> For each opportunity on the page, give me a Markdown table with: Title, Agency, Solicitation Number, Posted Date, Response Deadline, NAICS, and Link. Sort by Response Deadline (soonest first).

:::{note}
Path A works, but it's brittle — SAM.gov changes its UI, and JavaScript-rendered pages are harder for Firecrawl to parse cleanly. **Path B is significantly more reliable and only takes ~15 minutes to set up.** If you'll be using this daily, do Path B.
:::

### Path B — Free SAM.gov API key (recommended, ~15 minutes one-time setup)

Clean JSON data, stable URL structure that won't break when SAM.gov redesigns their UI. The API key is **free** and tied to a personal login.gov account — this is *not* the same as SAM.gov entity registration (which takes 7-10 days). The personal API key is generated immediately.

**Step-by-step: Get the free API key**

1. Go to <https://sam.gov> and click **Sign In** (top right).
2. If you don't have an account, click **Create User Account** (uses login.gov — free, just an email + password + 2FA).
3. Once signed in, click your name in the top-right corner → **Account Details**.
4. Scroll to the **API Key** section and click **Generate API Key**.
5. Enter your account password when prompted and copy the key that appears.
6. **Save the key somewhere safe right now** — once you navigate away from the page, SAM.gov won't show it to you again.

The public API key allows ~1,000 requests per day — way more than enough for a daily opportunity hunt.

**Try it now (with API key)**

Paste this into the Agent panel, replacing `YOUR_KEY`:

> Use Firecrawl to fetch this URL and parse the JSON response:
> `https://api.sam.gov/prod/opportunities/v2/search?api_key=YOUR_KEY&postedFrom=04/01/2026&postedTo=04/28/2026&typeOfSetAside=SDVOSBC&naicsCode=541330&limit=25`
>
> Give me a Markdown table of: Title, Department/Agency, Solicitation Number, Posted Date, Response Deadline, Place of Performance, and Link (build links as `https://sam.gov/opp/{noticeId}/view`). Sort by Response Deadline.

**Common set-aside codes for the `typeOfSetAside` parameter:**

| Code | Description |
|------|-------------|
| `SDVOSBC` | SDVOSB Set-Aside (competed) |
| `SDVOSBS` | SDVOSB Sole Source |
| `8AC` | 8(a) Set-Aside (competed) |
| `8A` | 8(a) Sole Source |
| `SBA` | Total Small Business Set-Aside |
| `WOSB` | Women-Owned Small Business |
| `HZC` | HUBZone Set-Aside |

### The skill

Save as `.agents/skills/daily-opportunity-hunter/SKILL.md`:

```markdown
---
name: daily-opportunity-hunter
description: Scan SAM.gov daily for new federal contract opportunities matching Lukos's NAICS codes and set-asides. Produces a ranked digest with bid-fit scores. Use when the user asks for new RFPs, the morning ops digest, or "what's new on SAM today."
---

# Skill: Daily SAM.gov Opportunity Hunter

## Triggers
Use this skill when the user says any of: "run the opportunity hunter,"
"check SAM.gov today," "what's new on SAM," "morning ops digest,"
"any new RFPs," or asks for new federal opportunities.

## Required tools
- Firecrawl

## Optional tools
- Memory (recall previously-seen opportunities)
- Sequential Thinking (scoring logic)

## Search profile
NAICS, set-asides, and target agencies are defined in GEMINI.md.
Default date window: last 7 days unless user specifies otherwise.

## Workflow
1. Determine which path is configured. If a SAM.gov API key is provided,
   use Path B. Otherwise use Path A.
2. **Path A (public scrape):** Ask the user to provide a SAM.gov search URL
   they generated by applying filters at sam.gov/opportunities.
   Use Firecrawl on that URL. Note: less reliable than Path B.
3. **Path B (API):** Use Firecrawl on
   `https://api.sam.gov/prod/opportunities/v2/search?api_key={KEY}&postedFrom={date}&postedTo={today}&typeOfSetAside={setAside}&naicsCode={code}&limit=50`
4. De-duplicate results (same Solicitation Number = duplicate).
5. If Memory is available, drop any opportunity already seen.
6. For each remaining opportunity, score 1-5 on fit:
   - Set-aside matches Lukos eligibility
   - NAICS in core capability list (high score) vs adjacent
   - Response deadline reasonable (>14 days = better)
   - Agency in target list from GEMINI.md
7. Output a Markdown table sorted by score descending, then by deadline
   ascending. Columns: Score, Title, Agency, Sol#, Posted, Deadline,
   NAICS, Set-Aside, Link, One-line rationale.
8. Save the digest to `digests/sam-{YYYY-MM-DD}.md`.
9. If Memory is available, record reviewed Solicitation Numbers.

## Output format
A single Markdown table. No preamble. End with a one-line summary:
"Reviewed N new opportunities. Top 3 to look at: [titles]."
```

---

## Use Case 2: RFP Response Co-Drafter

### What it does

You drop an RFP PDF into the `rfps/active/` folder. The agent reads the entire Statement of Work, identifies evaluation criteria, pulls the most relevant past performance from your library, looks up FAR clauses cited, and produces a section-by-section response outline. Doesn't write the whole proposal — gives your team a strong starting skeleton.

### Tools needed

- **Mandatory:** Sequential Thinking
- **Optional:** Memory (past performance retrieval), Firecrawl (live FAR clause text)

### Path A — No account, no API key

Antigravity's built-in file access reads the RFP from your workspace. Sequential Thinking is purely local. Past performance lives in Memory if you've built it (see Use Case 3).

**Try it now**

1. Save an RFP PDF to `rfps/active/` (e.g., `rfps/active/uewtep-iii.pdf`).
2. Paste this into the Agent panel:

> Read the RFP at `rfps/active/uewtep-iii.pdf`. Then use Sequential Thinking to:
>
> 1. Identify the agency, solicitation number, response due date, and total ceiling value.
> 2. Extract the Statement of Work — list every required task as a bullet.
> 3. Pull out the evaluation factors and their relative weights.
> 4. List every FAR or DFARS clause cited.
> 5. Produce a Word-ready outline of our response with placeholder sections matching the agency's required structure.
> 6. For each section, suggest which Lukos past performance would be most relevant and why.
>
> Save the output as a Markdown document in `rfps/active/uewtep-iii-outline.md`.

### Path B — Add live FAR clause lookup

Same skill, but when the agent finds a FAR clause it doesn't recognize, Firecrawl pulls the current text from acquisition.gov. No account or key needed — acquisition.gov is fully public.

### The skill

Save as `.agents/skills/rfp-co-drafter/SKILL.md`:

```markdown
---
name: rfp-co-drafter
description: Read an RFP PDF and produce a section-by-section response outline with extracted requirements, evaluation criteria, and recommended past performance citations. Use when the user drops an RFP into rfps/ or says "draft a response," "break down this solicitation," or "build the proposal outline."
---

# Skill: RFP Response Co-Drafter

## Triggers
Use this skill when the user says: "draft a response to this RFP,"
"break down this solicitation," "build the proposal outline,"
"start the proposal for [filename]," or points the agent at an
RFP PDF in rfps/active/.

## Required tools
- Sequential Thinking

## Optional tools
- Memory (past performance retrieval)
- Firecrawl (live FAR clause text)

## Workflow
1. Read the RFP file from the path the user provides
   (typically in `rfps/active/`).
2. Extract metadata: agency, solicitation number, contract vehicle,
   NAICS, set-aside, response deadline, period of performance,
   ceiling value, place of performance.
3. Extract the Statement of Work as a numbered list of tasks.
4. Extract Section L (Instructions) and Section M (Evaluation).
   List every evaluation factor with its weight.
5. List every FAR/DFARS clause cited. If Firecrawl is available,
   for each clause look up `https://www.acquisition.gov/far/{clause-number}`
   and summarize the obligation in plain English.
6. If Memory is available, search past performance for matches:
   same agency, similar scope, similar dollar value, recent (last 5 years).
7. Produce a response outline mirroring Section L's required structure.
   Include placeholders for:
   - Executive Summary
   - Technical Approach (one subsection per major SOW task)
   - Management Approach
   - Past Performance (with 3 specific recommended citations)
   - Pricing approach (high-level only)
8. For each technical subsection, write a one-paragraph "win theme"
   suggestion based on Lukos's strengths (per GEMINI.md).
9. Save the outline to `rfps/active/{rfp-name}-outline.md`.

## Output format
A single Markdown document with these sections:
- Cover sheet (metadata)
- Compliance matrix (table: requirement | section | response location)
- Response outline (the actual skeleton)
- FAR clause obligations summary
- Past performance recommendations (with rationale)
- Open questions for the capture team
```

---

## Use Case 3: Past Performance Knowledge Base

### What it does

This is the foundation that makes Use Cases 1, 2, 5, and 7 dramatically better. Every contract Lukos has completed gets logged in Memory with structured fields. When any future RFP asks "describe a similar effort," you can pull the right three citations in seconds instead of digging through old proposals.

### Tools needed

- **Mandatory:** Memory
- **Optional:** None

### Path A — No account, no API key

Memory runs entirely locally. Nothing to sign up for.

**Step 1 — Set up the structure (do this once)**

> Use Memory to create a knowledge graph schema for Lukos past performance. For each project, store these fields:
>
> - Project name
> - Contract or task order number
> - Agency / customer
> - Period of performance (start, end)
> - Total contract value
> - Lukos role (prime / subcontractor)
> - Scope summary (3-5 sentences)
> - Key personnel placed
> - Outcome / metrics achieved
> - Customer reference (name, title, can-be-contacted: yes/no)
> - Lessons learned
> - Tags (e.g., "USSOCOM", "training", "medical", "8(a)", "SDVOSB")
>
> Confirm the schema is set up and ready to receive entries.

**Step 2 — Add a project (do this for each completed contract)**

> Add a past performance entry to Memory:
>
> Project: USSOCOM Enterprise Wide Training and Exercise Program (UEWTEP) II
> Contract number: H92222-18-R-0001
> Agency: USSOCOM
> [continue filling in fields...]

**Step 3 — Query when needed (any time after)**

> Search Memory for past performance matching: USSOCOM, training, last 7 years. Give me the top 3 in a Markdown table with columns: Project, Agency, Period, Value, Customer Reference, Why-it-matches.

### The skill

Save as `.agents/skills/past-performance-kb/SKILL.md`:

```markdown
---
name: past-performance-kb
description: Maintain Lukos's past performance knowledge base in Memory. Add new completed contracts as structured entries; search the library when an RFP asks for similar past performance. Use when the user says "add past performance," "log a project," "find similar PP," or "give me citations for [topic]."
---

# Skill: Past Performance Knowledge Base

## Triggers
Use this skill when the user says: "add past performance," "log a project,"
"find similar past performance," "search PP," "what have we done with
[agency]," or "give me citations for [topic]."

## Required tools
- Memory

## Modes
- **Setup mode:** First run only. Establish the schema.
- **Add mode:** User is providing a new project to log.
- **Search mode:** User is asking for past performance to cite.

## Schema (entity type: PastPerformance)
Each entity must have observations covering: name, contractNumber, agency,
periodStart, periodEnd, value, role (prime|subcontractor), scope,
keyPersonnel, outcomeMetrics, customerReference, lessonsLearned, tags.

## Add mode workflow
1. Parse the user's input and extract every field above.
2. If a field is missing, ask the user once for it. Do not invent values.
3. Create a new entity in Memory with type "PastPerformance".
4. Create relationships to Agency entities (creating them if they don't exist).
5. Confirm the addition with a one-line summary.

## Search mode workflow
1. Identify search criteria: agency, topic, time range, dollar range, role.
2. Query Memory for PastPerformance entities matching all criteria.
3. Rank by recency (newer = higher) and dollar value match.
4. Return top 3 as a Markdown table:
   Project | Agency | Period | Value | Reference | Why-it-matches.
5. Append: "Recommendation: cite [project] first because [reason]."

## Output format
- Add mode: One confirmation line.
- Search mode: Markdown table + recommendation line.
```

---

## Use Case 4: FAR / DFARS Clause Researcher

### What it does

When an unfamiliar FAR or DFARS clause appears in a solicitation, the agent fetches the current authoritative text from acquisition.gov, summarizes the obligation in plain English, flags whether it's been amended in the last 12 months, and lists the typical compliance evidence required.

### Tools needed

- **Mandatory:** Firecrawl
- **Optional:** Brave Search (helpful if you only have a partial clause name), Sequential Thinking (cleaner summaries), Memory (cache common clauses)

### Path A — No account, no API key

Acquisition.gov is fully public, no login of any kind. Works out of the box.

**Try it now**

> Use Firecrawl to fetch `https://www.acquisition.gov/far/52.204-21` and give me:
>
> 1. The official clause title
> 2. A 3-sentence plain-English summary of what it requires
> 3. The compliance evidence a contracting officer typically wants to see
> 4. Whether this clause was amended in the last 12 months (look for the effective date in the clause header)

If you don't know the exact URL pattern, use Brave Search first:

> Use Brave Search to find the acquisition.gov URL for "FAR 52.204-21 basic safeguarding of covered contractor information." Then use Firecrawl on that URL and give me the plain-English summary.

### Path B

There is no Path B. Acquisition.gov has no API and requires no key. Path A is the only path, and it's free forever.

### The skill

Save as `.agents/skills/far-clause-researcher/SKILL.md`:

```markdown
---
name: far-clause-researcher
description: Look up FAR or DFARS clauses on acquisition.gov and produce plain-English compliance summaries with required evidence. Use when the user says "look up FAR [number]," "what does DFARS [number] mean," or "explain this clause."
---

# Skill: FAR / DFARS Clause Researcher

## Triggers
Use this skill when the user says: "look up FAR [number],"
"what does DFARS [number] mean," "explain this clause,"
"compliance summary for [clause]," or pastes a clause number.

## Required tools
- Firecrawl

## Optional tools
- Brave Search (if exact clause number is unclear)
- Sequential Thinking (for cleaner synthesis)
- Memory (to cache previously-researched clauses)

## Workflow
1. Identify the clause number from the user's input.
2. If Memory is available, check for a cached version less than 30 days old.
3. Fetch the clause from acquisition.gov:
   - FAR clauses: `https://www.acquisition.gov/far/{number}`
   - DFARS clauses: the URL includes the clause title slug — use Brave Search
     with `site:acquisition.gov dfars {number}` to find the exact URL.
4. Extract: official title, effective date, any recent amendment date, full text.
5. Produce a plain-English summary:
   - **What it requires** (1-2 sentences)
   - **Who it applies to** (which contractors, which contract types)
   - **What evidence the CO will want** (specific deliverables or certifications)
   - **Recently amended?** (yes/no — if yes, summarize what changed)
6. If Memory is available, cache the result with the URL as key.

## Output format
A single Markdown block with the four sections above. No preamble.
End with: "Source: {url}, retrieved {date}."
```

---

## Use Case 5: Pre-Meeting Intelligence Brief

### What it does

Before any meeting with a contracting officer, program manager, customer, or industry partner, the agent produces a one-page brief: who they are, recent moves, the agency's recent contract awards, budget priorities, news from the last 30 days, leadership changes. With Memory enabled, the brief gets richer every time you meet with the same person.

### Tools needed

- **Mandatory:** Web search (DuckDuckGo for free; Brave Search for higher-quality results), Firecrawl
- **Optional:** Memory (relationship history), Sequential Thinking

### Path A — DuckDuckGo, fully free, no account

DuckDuckGo Search runs locally through its MCP server with no key, no signup, no credit card. Result quality is slightly lower than Brave on government/technical queries, but for most pre-meeting research it's good enough.

**Try it now**

> Use DuckDuckGo Search to research:
>
> 1. "[Person name] [their organization]" — find their role and recent activity
> 2. "[Agency] recent contract awards 2026"
> 3. "[Agency] training and exercise priorities 2026"
>
> For each search, use Firecrawl to read the top 2-3 results.
>
> Then give me a one-page meeting brief in this format:
>
> **THE PERSON** — Current role, recent moves, relevant background
> **THE AGENCY/UNIT** — Recent awards (90 days), stated priorities, budget context
> **TALKING POINTS** — 3 things to bring up, 2 things to avoid
> **OPEN QUESTIONS** — Things we don't know yet that would help

### Path B — Brave Search API (paid, but cheap — ~$5/month for typical use)

:::{note}
As of February 2026, Brave replaced their free tier with metered billing — every plan now requires a credit card and includes $5 in monthly credits, which covers ~1,000 queries. Beyond that, queries are billed at $5 per 1,000. For Lukos's typical pre-meeting research volume (maybe 20-50 queries per brief), the included credits will cover it, but the credit card is still required at signup. Brave's signal-to-noise is meaningfully better than DuckDuckGo on technical/government queries.
:::

**Step-by-step: Get the Brave API key**

1. Go to <https://brave.com/search/api/> and click **Get Started**.
2. Create an account (email + password).
3. Add a credit card (required for all plans).
4. Subscribe to the **Search** plan ($5 in monthly credits applied automatically).
5. Generate an API key from the dashboard.
6. Fill in `"BRAVE_API_KEY": "your-key-here"` in your `mcp_config.json`.

### The skill

Save as `.agents/skills/pre-meeting-brief/SKILL.md`:

```markdown
---
name: pre-meeting-brief
description: Produce a one-page intelligence brief before any meeting with a federal contracting officer, program manager, or industry partner. Covers the person, the agency, recent context, talking points, and open questions. Use when the user says "brief me before my meeting with [name]" or "intel on [person/agency]."
---

# Skill: Pre-Meeting Intelligence Brief

## Triggers
Use this skill when the user says: "brief me before my meeting with [name],"
"intel on [person/agency]," "prep for my [date] call with [person],"
or "what should I know about [person/agency]."

## Required tools
- Brave Search OR DuckDuckGo Search (Brave preferred if available)
- Firecrawl

## Optional tools
- Memory (recall prior briefs about the same person/agency)
- Sequential Thinking

## Workflow
1. Identify the meeting subject(s): person name, agency/unit, meeting purpose.
2. If Memory is available, retrieve any existing entry for this person/agency.
3. Run searches:
   a. "{person name} {their organization}" — LinkedIn, bio, quotes
   b. "{agency} recent contract awards" — last 90 days
   c. "{agency} priorities {current year}" — strategic direction
   d. "{agency} {meeting topic if known}" — specific context
4. Use Firecrawl to read the top 2-3 results from each search.
5. Synthesize into the brief format below.
6. If Memory is available, save new findings under the person/agency entity.

## Output format

### THE PERSON
- **Role:** Current title and reporting line
- **Background:** 2-3 sentences of relevant career history
- **Recent moves:** Anything in the last 6 months

### THE AGENCY / UNIT
- **Recent awards (90 days):** Bullet list with values and awardees
- **Stated priorities:** From official sources, not press
- **Budget context:** Any relevant funding signals

### TALKING POINTS
1-3 things to bring up (specific, sourced)
1-2 things to avoid

### OPEN QUESTIONS
Things we don't know yet that could be asked respectfully

End with: "Sources: [list of URLs used]"
```

---

## Use Case 6: Veteran Talent Sourcing & Outreach

### What it does

Given an open requisition (e.g., "Joint Fires Project Manager, NSW background"), the agent searches public sources for veteran candidates with matching military experience, drafts a personalized outreach message in Lukos's voice, and logs each candidate in Memory for follow-up.

### Tools needed

- **Mandatory:** Brave Search, Firecrawl
- **Optional:** Memory (candidate pipeline tracking)

### Path A — No account, no API key

Use DuckDuckGo Search instead of Brave. Firecrawl is the only thing needing a free key.

**Try it now**

> Use Brave Search (or DuckDuckGo) to find candidate profiles for this role:
>
> **Role:** Joint Fires Project Manager
> **Required background:** Naval Special Warfare (NSW) operator, 8+ years experience
> **Location preference:** Tampa, FL or Virginia Beach, VA
> **Search queries to run:**
> - "Naval Special Warfare project manager LinkedIn site:linkedin.com/in"
> - "former Navy SEAL project manager NSW"
> - "Joint Fires SME veteran LinkedIn"
>
> For each top result, use Firecrawl to read the public profile (only public, no login). Give me a Markdown table with: Name, Current Role, Military Background, Location, Public Profile URL, and Fit Score (1-5).
>
> Then for the top 3, draft a short LinkedIn-style outreach message (under 100 words each) in our Wolfpack voice — peer-to-peer, never corporate.

### Path B

Same as Use Case 5 — set up a Brave Search API key (~$5/month metered) for better-quality candidate searches.

### The skill

Save as `.agents/skills/veteran-talent-sourcing/SKILL.md`:

```markdown
---
name: veteran-talent-sourcing
description: Find veteran candidates for open Lukos roles using public sources, score them on fit, and draft personalized outreach messages in the Wolfpack voice. Use when the user says "find candidates for [role]," "source for [position]," or pastes a job description.
---

# Skill: Veteran Talent Sourcing & Outreach

## Triggers
Use this skill when the user says: "find candidates for [role],"
"source for [position]," "who could fill this req," "draft outreach
for [role]," or pastes a job description.

## Required tools
- Brave Search OR DuckDuckGo Search
- Firecrawl

## Optional tools
- Memory (candidate pipeline tracking)

## Workflow
1. Parse the requisition: role title, required background, preferred location, key skills.
2. Build 3-5 search queries combining role keywords and military service terms.
   Always include public site filters (e.g., `site:linkedin.com/in/`).
3. Run searches and collect URLs. Aim for 10-15 candidate URLs.
4. Use Firecrawl to read each public profile (public pages only — never log in).
5. Score each candidate 1-5 on fit:
   - Military background match (0-2 points)
   - Years of relevant experience (0-1)
   - Location preference (0-1)
   - Recent activity / availability signals (0-1)
6. Output a Markdown table sorted by score:
   Name, Current Role, Military Background, Location, URL, Fit Score, Notes.
7. For the top 3, draft personalized outreach messages (under 100 words each):
   - Warm, peer-to-peer tone — never corporate-recruiter
   - Reference something specific from their public profile
   - Mention Lukos's veteran-first culture and "the Wolfpack"
   - End with a low-friction ask (15-min call)
8. If Memory is available, log each candidate as an entity with status "sourced".

## Important constraints
- Use only PUBLIC information. Never attempt to log in to LinkedIn.
- Do not invent details. If their public profile doesn't say something, don't include it.

## Output format
1. Candidate ranking table (Markdown)
2. Top 3 outreach drafts (each in a code block for easy copy)
3. Optional: "Logged in Memory" confirmation if available
```

---

## Use Case 7: USAspending.gov Competitor & Awardee Watch

### What it does

When a competitor wins a contract you cared about, the agent grabs the award announcement, pulls the full contract record from USAspending.gov, and logs it. The big payoff: 12 months before the contract expires, the skill surfaces it as a re-compete opportunity worth chasing.

### Tools needed

- **Mandatory:** Firecrawl
- **Optional:** Memory (track competitors over time), Brave Search (find the original announcement), Sequential Thinking (analyze patterns)

### Path A — Fully public, no account ever needed

USAspending.gov is one of the cleanest data sources in federal contracting. **The API requires no key, no account, no signup at all.** Just call URLs.

**Try it now — Look up a specific awardee**

> Use Firecrawl to make a POST request to:
>
> `https://api.usaspending.gov/api/v2/search/spending_by_award/`
>
> with this JSON body:
>
> ```json
> {
>   "filters": {
>     "recipient_search_text": ["Wittenberg Weiner Consulting"],
>     "time_period": [{"start_date": "2024-01-01", "end_date": "2026-12-31"}],
>     "award_type_codes": ["A","B","C","D"]
>   },
>   "fields": ["Award ID","Recipient Name","Awarding Agency","Award Amount","Period of Performance Start Date","Period of Performance Current End Date","Description"],
>   "page": 1,
>   "limit": 25,
>   "sort": "Award Amount",
>   "order": "desc"
> }
> ```
>
> Give me the results as a Markdown table sorted by Award Amount descending.

:::{note}
If your version of Firecrawl can't do POST requests, use the search UI on <https://www.usaspending.gov> directly (also no login). Then point Firecrawl at the resulting URL.
:::

**Try it now — Find competitors for a specific NAICS**

> Use Firecrawl to POST to `https://api.usaspending.gov/api/v2/search/spending_by_award/` with:
>
> ```json
> {
>   "filters": {
>     "naics_codes": {"require": [["541330"]]},
>     "time_period": [{"start_date": "2025-01-01", "end_date": "2026-04-28"}],
>     "set_aside_type_codes": ["SDVOSBC"],
>     "award_type_codes": ["A","B","C","D"]
>   },
>   "fields": ["Recipient Name","Award Amount","Awarding Agency","Award ID","Period of Performance Current End Date"],
>   "page": 1,
>   "limit": 50,
>   "sort": "Award Amount",
>   "order": "desc"
> }
> ```
>
> Aggregate by Recipient Name and give me the top 10 SDVOSB awardees in NAICS 541330 by total dollars won.

### Path B

There is no Path B. Path A IS the public path. This is the cleanest no-friction data source you'll ever work with.

### The skill

Save as `.agents/skills/competitor-awardee-watch/SKILL.md`:

```markdown
---
name: competitor-awardee-watch
description: Track competitor awards on USAspending.gov, scan top awardees by NAICS, and surface contracts expiring in the next 12 months as re-compete opportunities. Use when the user says "who won [contract]," "track [competitor]," "competitors in NAICS [code]," or "re-competes coming up."
---

# Skill: USAspending.gov Competitor & Awardee Watch

## Triggers
Use this skill when the user says: "who won [contract]," "track [competitor],"
"what did [company] win recently," "competitors in NAICS [code],"
"re-competes coming up," or "find expiring contracts for [agency]."

## Required tools
- Firecrawl (must support POST requests with JSON body)

## Optional tools
- Memory (build a persistent competitor watch list)
- Brave Search (context on awards)
- Sequential Thinking (pattern analysis)

## Modes
- **Awardee lookup:** Pull all recent awards for a specific company
- **NAICS competitor scan:** Find top awardees in a NAICS code
- **Agency awards:** Find recent awards from a specific agency
- **Re-compete watch:** Find contracts expiring in next 12 months

## API endpoint (all modes)
- URL: `https://api.usaspending.gov/api/v2/search/spending_by_award/`
- Method: POST
- **No authentication required.**
- Award type codes: A=BPA Call, B=Purchase Order, C=Delivery Order, D=Definitive Contract

## Awardee lookup workflow
1. POST with filter `recipient_search_text: [companyName]` and time_period
   covering user's range (default last 24 months).
2. Output a Markdown table: Award ID, Agency, Amount, POP Start, POP End, Description.
3. If Memory is available, save the awardee with all award IDs.

## NAICS competitor scan workflow
1. POST with filter `naics_codes: {require: [[code]]}` plus the user's
   time_period (default last 24 months).
2. Optionally filter by set_aside_type_codes if user specifies.
3. Aggregate results by Recipient Name; sum Award Amount.
4. Output top N as a table: Rank, Recipient, Total $, # Awards, Most Common Agency.

## Re-compete watch workflow
1. POST with the user's filter plus time_period looking forward — match contracts
   where Period of Performance Current End Date is within 12 months.
2. Sort ascending by end date.
3. Output: Awardee, Agency, Total Value, End Date, Days Until Expiry, Original Award ID.
4. Append: "Top re-compete to track: [contract] (expires {date}, value $X)."

## Output format
- Always Markdown tables, no preamble.
- Always include the API URL used at the bottom for transparency.
- If Memory is available, end with one line confirming what was logged.
```

---

## Putting it all together

Once you have the Past Performance KB (Use Case 3) populated, here's the rhythm:

**Every morning (5 minutes):**
Daily Opportunity Hunter (#1) → triage new RFPs.

**Every Friday afternoon (30 min):**
Re-compete Watch mode of #7 → contracts expiring in 12 months.
NAICS Competitor Scan for top NAICS → see who's eating your lunch.

**When an RFP looks promising:**
RFP Co-Drafter (#2) → starting outline.
Each FAR clause flagged → FAR Clause Researcher (#4).

**Before any external meeting:**
Pre-Meeting Brief (#5) → never walk in cold.

**Whenever a relevant role opens up:**
Veteran Talent Sourcing (#6).

---

## How to write your own skills

You now have eight working examples (the setup skill plus seven workflows). The pattern is always the same:

1. **YAML frontmatter** — `name` (slug, no spaces) and `description` (one sentence the agent uses to decide when to load this skill).
2. **Triggers** — the natural-language phrases that should fire this skill.
3. **Required tools** — the minimum set. Be ruthless. Fewer = more reliable.
4. **Optional tools** — the "nice to have" set that enriches output if available.
5. **Workflow** — numbered steps. Be specific about URLs, fields, criteria.
6. **Output format** — exactly what you expect back.
7. **Constraints** — anything the agent should NOT do.

When you write your ninth skill, copy one of the existing ones and modify it. After three or four, you'll see what makes them robust:

- **Specific over vague.** "Sort by Response Deadline ascending" beats "sort sensibly."
- **Explicit about missing data.** Tell the agent what to do when Memory is empty.
- **Define the output up front.** A well-defined output is half the skill.
- **One skill, one job.** If a skill has two unrelated modes, split it.

---

*Last updated April 2026.*
