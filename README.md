### Jadd Al-Khabbaz

AI engineer and Computer Science graduate (University of Liverpool), based in Liverpool. I build LLM products, the infrastructure under them, and the agent systems that run my own web studio.

**umbo: a WhatsApp assistant that runs the business**

[umbo](https://www.umbo.uk) is my own SaaS: a booking system and loyalty engine that connects the bookings, till and bank a small business already uses. Its agent layer is a WhatsApp assistant. An owner or their staff texts it the way they'd text a colleague ("move Sam to 3, cancel Dan, how did we do this week?") and it does the work.

- **A pipeline, not a single prompt.** A fast classifier model reads each message first and decides what kind of turn it is. Claude splits a message into separate requests, the classifier routes each one to a capability, code resolves names and times against real data, and every write is checked before it runs. Claude then drafts one reply covering everything.
- **Built to be trusted with real customers.** Every action can be undone by replying UNDO. Messages that arrive mid-turn fold into the same reply. Safety checks catch messages that aren't from the owner, requests to move money or pull data, and prompt injection, and answer them with fixed replies.
- **Tested like software.** There's an eval suite of recorded conversation threads, a turn queue with leases and idempotency keys so a retried turn never writes twice, and each new engine version ships behind a flag.

**LLM infrastructure**

- [**llm-gateway**](https://github.com/jadd0/llm-gateway): a self-hostable TypeScript gateway for LLM APIs. It exposes one OpenAI-compatible endpoint and handles routing, distributed rate limits, per-tenant budgets, provider fallback, and exact and semantic caching. It's in early development, and every number it publishes will come from a benchmark you can re-run.

**Agents that build the software**

- **Product loop:** umbo itself is built by a coordinator agent that works through its roadmap. It plans each PR, runs builder agents in parallel git worktrees, and sends their work through tests, a verifier agent and code review before merging.
- **Studio pipeline:** client websites are built by a multi-agent pipeline on Claude Code. A designer agent matches each brief against an audited corpus of reference sites and a bank of about 50 components, a builder implements the spec, and a render gate screenshots every page at 1280px and 390px before I sign it off. Recent sites: [Kanti Skincare](https://www.kantiskincare.com) and [Thornton Hough Village Club](https://www.thorntonhoughvillageclub.com).
- **Commerce engine:** Kanti runs on my own ecommerce engine instead of Shopify. It's Next.js, Postgres and Stripe, and it handles the catalogue, cart, checkout, customer accounts and returns. The engine has no branding of its own, and it's built so each new store can fork it, add its own theme and keep getting engine fixes from upstream.
- **Knowledge vault:** a nightly job audits a shared knowledge base, triages new notes and records what the agents learned, so each session starts from the last one.

**Stack:** TypeScript, Next.js, Postgres, Drizzle, the Claude API, WhatsApp.

More at [jadd.live](https://jadd.live)
