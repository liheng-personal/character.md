# CHARACTER.md

Most agent memory systems solve retrieval — how to store and fetch what happened in past conversations. None of them define *identity*: who the agent is, what it knows apart from conversation history, and how those different kinds of memory relate to each other.

CHARACTER.md is a structured identity spec for AI agents. It separates memory into three layers — dispositions (procedural), knowledges (semantic), and experiences (episodic) — aligning with the CoALA cognitive architecture's long-term memory taxonomy. One file, plain Markdown, no runtime dependency.

## Getting started

The fastest way to understand the format is to look at one. Here's a minimal example — a TRPG character named Aldric, in a single file:

[`examples/minimal/CHARACTER.md`](examples/minimal/CHARACTER.md)

Copy it into a conversation with any AI, and the AI will know who Aldric is, what he knows about his world, and what happened to him last week. Edit it, and you have your own agent.

## What you can build with it

**A persistent AI assistant.** Define how your agent should behave, write down what it needs to know about you and your work, and let it accumulate experiences over time. The next conversation picks up where the last one left off — not because the platform remembers, but because the file does. The format marks which parts are stable, which parts update, and which parts only grow — an AI consuming the file can write changes back to the right place automatically.

**A multi-agent team.** Each agent is a self-contained file. Load several into the same conversation and they each bring their own dispositions, knowledge, and history. A team of AI assistants can cover different specialties without stepping on each other. The file is the boundary.

**A stateful NPC or game character.** Define starting traits and knowledge, then let the experience layer fill in as players interact. The format already separates what changes from what doesn't, so you don't need to build that logic yourself.

## How it works

A CHARACTER.md file has three sections, each handling a different kind of memory:

**Dispositions** — how the agent behaves and what it can do. Rules, tendencies, skills, and boundaries that stay mostly stable. Think of them as the agent's operating system: personality, tone, areas of competence, how it responds under specific conditions. In CoALA terms, this is *procedural memory* — the explicit, portable kind (instruction sets), not the implicit kind (model weights).

**Knowledges** — what the agent knows. Some are facts that rarely change; others are current state that gets updated as things happen. Domain knowledge, situational awareness, and working context live here. This maps to CoALA's *semantic memory*.

**Experiences** — what the agent has been through. A record of events, decisions, and interactions. This section only grows — nothing is deleted, but older entries can settle into deeper storage, the way long-term memory works. This maps to CoALA's *episodic memory*.

```
CHARACTER.md
├── Dispositions      — procedural: how it behaves
├── Knowledges        — semantic: what it knows
└── Experiences       — episodic: what it's been through
```

The spec defines a semantic structure, not a file format. Markdown is the simplest implementation — plain text, no tooling, works everywhere — but the same three sections work in any document your AI can read.

## How is this different from existing memory solutions?

Agent memory frameworks — Mem0, Zep, LangMem, Letta, and others — are runtime infrastructure. They solve how to store, index, and retrieve memories while your agent is running. Most cover one or two of CoALA's memory types well, but none of them define *what the memory should contain* or *how different memory types relate to each other* at the specification level.

CHARACTER.md operates at a different layer. It is an identity specification — it defines who the agent is *before* it starts running. The structure itself enforces the separation that memory frameworks leave to the developer: dispositions that rarely change, knowledge that updates in place, and experiences that only accumulate.

The two layers are complementary, not competing. You can use CHARACTER.md to define your agent's identity and plug any memory framework underneath for runtime retrieval.

CLAUDE.md and AGENTS.md serve a similar purpose for coding agents — persistent instructions and learned behaviors across sessions. They cover procedural and semantic memory well, but lack a structured episodic layer. CHARACTER.md adds that third layer and formalizes the boundaries between all three.

## Where to go from here

Start with the [minimal example](examples/minimal/CHARACTER.md) and adapt it to your agent.

To understand why the format is designed this way — why three sections, why natural language, why not YAML — read the [design rationale](RATIONALE.md).

If you're building a tool or platform that consumes character files, the [spec](SPEC.md) has the precise definitions, conformance rules, and lifecycle semantics.

For a working example of a CHARACTER.md served over MCP, see [book-mcp](https://github.com/liheng-personal/book-mcp).

## License

This specification is released under [CC BY 4.0](LICENSE). Use it, build on it, adapt it — just give credit.

## Credits

CHARACTER.md is created and maintained by [Narrativesaw](https://narrativesaw.com).
