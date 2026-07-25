# Quiz Topic Tree — Claude Code, Harness & Model Knowledge

Phase-1 deliverable: the topic taxonomy only. No questions yet.

## How to read this tree

**Levels** — each topic lists the span of difficulty levels it can produce questions for:

| Level | Name | Who it targets |
|---|---|---|
| L1 | Beginner | Has barely or never used Claude Code; knows "it's an AI coding tool" |
| L2 | Practitioner | Daily user; fluent with the product surface (commands, modes, files) |
| L3 | Mechanic | Understands the machinery: the loop, context assembly, caching, cost |
| L4 | Expert | Internals: protocol details, cache economics, provider deltas, model architecture |

**Bedrock delta** — per the framing requirement (user primarily on AWS Bedrock, sometimes Anthropic direct):

- `●` major difference — questions here should explicitly contrast Bedrock vs Anthropic
- `◐` some difference — worth a variant question or a "gotcha" distractor
- `○` no meaningful difference — provider-neutral questions

---

## 1. The Big Picture `○` (L1–L3)

The entry branch — orients beginners, but L3 questions test whether the pieces connect correctly.

- **1.1 What Claude Code is** `○` (L1) — CLI agent vs chatbot vs IDE plugin; what "agentic" means
- **1.2 The four layers** `◐` (L1–L2) — CLI → harness → API → model; which layer does what
- **1.3 The stateless loop** `○` (L2–L3) — the keystone: nothing lives server-side between turns; the harness rebuilds and resends the entire conversation every turn; why the model "forgets" nothing and remembers nothing
- **1.4 One turn, end to end** `◐` (L3) — user input → prompt assembly → API call → response blocks → tool execution → resend; where each general topic sits on that path
- **1.5 Usage patterns** `○` (L1–L2) — interactive vs headless/CI (`-p`), plan mode workflows, when to use Claude Code vs claude.ai

## 2. The CLI `○` (L1–L2)

Product-surface knowledge. Mostly provider-neutral; the few deltas are about login/model selection.

- **2.1 Slash commands** `◐` (L1–L2) — `/help`, `/clear`, `/compact`, `/model`, `/config`, `/init`, `/login` (Bedrock users don't `/login` — auth comes from AWS credentials)
- **2.2 Modes** `○` (L1–L2) — default, plan mode, auto-accept; permission modes; what changes in each
- **2.3 Keyboard shortcuts** `○` (L1–L2) — interrupt, mode cycling, history, multiline input, `!` shell prefix
- **2.4 Useful built-in skills & commands** `○` (L2) — code review, PR review, init, memory/remember-style workflows; skills vs built-in commands distinction
- **2.5 Invocation & flags** `◐` (L2) — `claude -p`, `--resume`/`--continue`, model selection flags, env-var driven config (where Bedrock users live)

## 3. The Harness — the machinery `◐` (L2–L4)

The core branch. Provider-neutral mechanics, but a few sub-topics shift under Bedrock.

- **3.1 Prompt assembly** `○` (L2–L4)
  - 3.1.1 The layered build: system prompt → tool schemas → context files → history → current turn
  - 3.1.2 The stability gradient: stable prefix first, volatile content last — and why that ordering is load-bearing (caching)
  - 3.1.3 Injection is selection, not a dump: what gets pulled in per turn (skills, memory, reminders) vs what's always there
  - 3.1.4 The transcript is not the message array — durable record ≠ what the model saw
- **3.2 The tool protocol** `○` (L2–L4)
  - 3.2.1 Tool use as structured-text round-trip: `tool_use` block out, `tool_result` block back in
  - 3.2.2 System tools vs custom/MCP tools — who defines the schema, who executes
  - 3.2.3 Why prompt injection is a protocol-level security category, not a bug
- **3.3 The special local files** `○` (L1–L3)
  - 3.3.1 `CLAUDE.md` (project / user / directory scoping) and rules files
  - 3.3.2 `settings.json` tiers: user, project, local — and precedence
  - 3.3.3 `~/.claude/` on-disk inventory: what lives where, durable vs ephemeral
  - 3.3.4 Transcript `*.jsonl`: schema, what's recorded, resume/continue mechanics
- **3.4 Configuration surfaces** `◐` (L2–L4) — two axes: *resolution* (which setting wins) and *landing* (does it change the request or only harness behavior); env vars vs settings files vs flags (Bedrock config is env-var heavy)
- **3.5 Permissions** `○` (L1–L3) — allow/deny rules, permission prompts, permission modes, sandbox behavior, tool allowlists
- **3.6 Context lifecycle & compaction** `○` (L2–L4)
  - 3.6.1 Why compaction must exist (context window is finite, resend grows every turn)
  - 3.6.2 What's kept, what's lost; summary + tail mechanics
  - 3.6.3 Auto-compact vs `/compact`; what compaction costs (a full read of the conversation)
  - 3.6.4 `/clear` vs compaction vs starting a new session
- **3.7 Mid-session changes** `○` (L3–L4) — what happens when you edit CLAUDE.md, settings, or MCP config mid-session; what re-resolves per turn vs per session; system-reminder injection
- **3.8 Skill injection mechanics** `○` (L3–L4) — descriptions always in context, bodies loaded on trigger; where in the assembly a skill lands; cost implications

## 4. Extension Points `◐` (L2–L4)

All four seams where users intervene in the loop, plus multi-agent constructs.

- **4.1 Hooks** `○` (L2–L4) — lifecycle events (pre/post tool use, session start, stop); hooks run in the harness, not the model; exit codes and blocking; hooks vs asking the model nicely
- **4.2 Skills** `○` (L2–L3) — what a skill is, frontmatter + description triggering, user vs project vs plugin skills, skills vs slash commands
- **4.3 Custom commands** `○` (L2) — command files, arguments, when a command beats a skill
- **4.4 MCP** `◐` (L2–L4) — servers as a third tool executor; transports (stdio, HTTP/SSE); config scopes; tool naming; auth to MCP servers; deferred tool loading; MCP works the same on Bedrock but the *server's own* model calls may not
- **4.5 Subagents** `○` (L2–L4) — separate loop with its own context, pass-by-value results; custom agent definitions; when to delegate vs do inline; context isolation as the point
- **4.6 Agent teams & orchestration** `◐` (L3–L4) — multi-agent patterns, parallel fan-out, worktree isolation; cost multiplication under fan-out (and Bedrock quota implications)
- **4.7 Dynamic workflows** `○` (L3–L4) — deterministic orchestration scripts over subagents vs model-driven delegation; when structure beats a single big context

## 5. The API Layer `●` (L2–L4)

The branch where Bedrock vs Anthropic matters most. Nearly every sub-topic has a provider delta.

- **5.1 Auth methods** `●` (L1–L3)
  - 5.1.1 Anthropic direct: API key, OAuth (Claude subscription plans)
  - 5.1.2 Bedrock: AWS credentials, IAM roles/policies, regions, `CLAUDE_CODE_USE_BEDROCK`
  - 5.1.3 Also on the map: Vertex AI; enterprise gateways/proxies
- **5.2 Model identity & availability** `●` (L2–L3) — Anthropic model names vs Bedrock model IDs / inference profiles; regional availability; new models and features arriving on Bedrock later
- **5.3 Prompt caching (the API view)** `●` (L2–L4)
  - 5.3.1 Cache breakpoints, prefix matching, cache-read vs cache-write vs uncached pricing
  - 5.3.2 5-minute vs 1-hour TTL — what each costs, when each applies, provider support differences
  - 5.3.3 What breaks the cache: any prefix edit, tool set change, system prompt change, TTL expiry
  - 5.3.4 KV cache: what is actually stored server-side (attention state, not text)
- **5.4 Rate limiting & quotas** `●` (L2–L4) — Anthropic rate limits/tiers vs Bedrock quotas and throughput models (on-demand vs provisioned); throttling behavior; retries
- **5.5 Costs** `●` (L1–L4)
  - 5.5.1 Token pricing model: input vs output vs cache read/write
  - 5.5.2 Why cost grows superlinearly across a session (the resend)
  - 5.5.3 Subscription (Max/Pro) vs API pay-per-token vs AWS billing
  - 5.5.4 Cost levers: caching, compaction, `/clear`, model choice, subagent isolation
- **5.6 Request/response anatomy** `◐` (L3–L4) — messages array, content blocks, stop reasons, streaming; where Bedrock wraps the same Anthropic-shaped payload (converse vs invoke-model style differences)

## 6. The Model `○` (L1–L4)

Provider-neutral: the same Claude weights answer either way. General concepts only — no local-LLM tooling specifics, but its general concepts (parameters, quantization, MoE) are fair game as examples.

- **6.1 What a model is** `○` (L1–L2) — weights as numbers in memory; parameters vs file size vs capability; context window as a hard budget
- **6.2 The read phase (prefill)** `○` (L3–L4) — parallel ingestion of the prompt; compute-bound; builds the KV cache; why input tokens are cheap and prefill explains time-to-first-token
- **6.3 The write phase (decode)** `○` (L3–L4) — one token at a time, each pass reading all prior KV state; memory-bandwidth-bound; why output tokens cost more and stream slowly
- **6.4 Architecture basics** `○` (L2–L4) — layers and the transformer stack (concept level); attention as "which earlier tokens matter"; embeddings/tokens vs words
- **6.5 Parameters & model size** `○` (L2–L3) — what "a 200B model" means; parameter count vs quality vs speed tradeoff; why bigger isn't automatically better for a task
- **6.6 Quantization** `○` (L2–L4) — fewer bits per weight; the quality/memory/speed triangle; size ≈ params × bits ÷ 8; why serving providers quantize and what it can cost in quality
- **6.7 MoE (mixture of experts)** `○` (L3–L4) — total vs active parameters; why a big model can run like a small one; what MoE changes about cost/latency reasoning
- **6.8 Sampling & stop conditions** `○` (L3–L4) — temperature, top-p (concept level); stop sequences, max tokens, stop reasons; why identical prompts differ
- **6.9 The Claude model family** `◐` (L1–L3) — tiers (Haiku/Sonnet/Opus and up), capability vs cost vs speed; choosing a model per task; availability per provider

## 7. Bedrock vs Anthropic — the cross-cutting axis `●` (L2–L4)

Every `●`/`◐` above yields contrast questions; this branch holds the deltas that deserve dedicated questions.

- **7.1 Setup & auth** (L2) — env vars vs `/login`; IAM vs API keys; region selection
- **7.2 Feature parity & lag** (L3) — which harness features are provider-independent (all of the CLI/harness machinery) vs API-dependent (caching TTLs, newest models, beta features)
- **7.3 Caching differences** (L3–L4) — support, TTL options, pricing structure differences
- **7.4 Throughput & limits** (L3–L4) — Bedrock quotas/provisioned throughput vs Anthropic tiers; different failure modes (throttling errors, region capacity)
- **7.5 Billing & governance** (L2–L3) — AWS billing/Cost Explorer vs Anthropic console; why enterprises pick Bedrock (VPC, compliance, data residency)
- **7.6 What never changes** (L2) — the anti-confusion topic: same model weights, same harness, same protocol; Bedrock is a different *door* to the same model

---

## Coverage checklist (vs quiz-topics.md)

| Source item | Tree node(s) |
|---|---|
| CLI: commands, modes, shortcuts | 2.1–2.5 |
| Harness: machinery, local files, tools, prompt assembly, protocol, *.jsonl, stability gradient, skill injection | 3.1–3.8 |
| API: auth, KV cache & TTL, rate limiting | 5.1, 5.3, 5.4 |
| Model: read phase, write phase, layers, parameters, quantization | 6.2–6.6 |
| High level architecture | 1.2–1.4 |
| Usage patterns | 1.5 |
| Costs | 5.5 |
| Auth methods | 5.1 |
| MCP | 4.4 |
| Skills | 4.2, 3.8 |
| Commands | 2.1, 4.3 |
| Hooks | 4.1 |
| Subagents | 4.5 |
| Agent teams | 4.6 |
| Dynamic workflows | 4.7 |
| Useful built-in skills & commands | 2.4 |
| Mid-session changes | 3.7 |
| Caching — 5m vs 1h, on disk | 5.3, 3.3.4 |
| Compaction | 3.6 |
| Permissions | 3.5 |
| Rules | 3.3.1, 3.5 |
| Bedrock vs Anthropic per topic | `●`/`◐` markers + branch 7 |
