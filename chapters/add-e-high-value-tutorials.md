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
