# Design Rationale

This document explains *why* CHARACTER.md is designed the way it is — the problem it solves, the cognitive science behind its memory model, and the trade-offs behind its key design decisions.

## The Problem

Imagine you're a game master using an AI to play an NPC in your tabletop campaign. You paste a paragraph of backstory into the system prompt: the character's name, personality, a few key relationships. The first few exchanges are great. Then, ten turns in, the character "forgets" a fact you established. Fifteen turns in, it contradicts its own personality. By turn twenty, you're spending more effort correcting the AI than playing the game.

This failure isn't random. It stems from a structural problem: the character definition is an unstructured blob of text. There's no separation between "how this character acts," "what this character knows," and "what this character has been through." The AI has no way to distinguish a behavioral rule from a trivia fact from a past event — so it treats them all the same, and they blur together as the conversation grows.

The same problem surfaces everywhere characters need to persist: AI agents that lose their personality across sessions, voice actors who receive inconsistent character guides, game designers whose NPC definitions don't survive the handoff between writers and programmers.

CHARACTER.md addresses this by giving characters **structured memory** — not more text, but text organized by what kind of memory it is.

## Why Three Kinds of Memory

A character needs to track three fundamentally different things:

**How they act** — behavioral rules, speech patterns, moral boundaries, professional procedures. These are relatively stable. A character who speaks formally doesn't suddenly switch to slang (unless something dramatic happens). A financial analyst who always cites sources doesn't stop citing them mid-conversation. These are the character's *dispositions*.

**What they know** — facts about the world, about themselves, about other characters. Some facts never change (the character's birthplace, the laws of their universe). Others shift as circumstances change (their current assignment, the status of a project, what they've learned recently). These are the character's *knowledges*.

**What happened to them** — events, encounters, decisions, consequences. This is inherently chronological or causal. Unlike facts, events have a narrative arc — they happen in sequence, and later events build on earlier ones. These are the character's *experiences*.

These three categories have fundamentally different properties. Dispositions are imperative ("do this when that happens"); knowledges are declarative ("X is Y"); experiences are narrative ("then this happened"). Dispositions change rarely; mutable knowledge changes as tasks progress; experiences only accumulate — you can archive old ones, but you don't edit them.

When all three are mixed into a single prompt or document, the system can't tell which is which. An event gets treated as a permanent fact. A rule gets softened by a nearby anecdote. Current state gets mixed up with historical state. Structured separation solves this by letting each memory type be stored, retrieved, and updated according to its own nature.

## The Cognitive Science Foundation

This three-way split is not arbitrary. It maps onto a well-established model in cognitive science: the distinction between **procedural memory** (knowing how), **semantic memory** (knowing what), and **episodic memory** (knowing when/where something happened).

CHARACTER.md specifically draws on **CoALA** — Cognitive Architectures for Language Agents (Sumers, Yao, Narasimhan & Griffiths, 2023), which adapts this cognitive framework to the architecture of AI agents. CoALA proposes that an effective language agent needs:

- **Working memory** — what is currently loaded into the context window. Small, expensive, immediately available.
- **Long-term memory** — persistent storage outside the context window. Large, cheap, retrieved on demand.

And within long-term memory, CoALA distinguishes:

- **Procedural memory** — action policies and behavioral rules.
- **Semantic memory** — factual knowledge about the world.
- **Episodic memory** — records of past events and interactions.

CHARACTER.md translates this directly into a file format. The main CHARACTER.md file is working memory — it's loaded at the start of every session. The three folders (dispositions/, knowledges/, experiences/) are long-term memory — their files are only pulled in when the agent needs them.

This means the format isn't just an organizational convenience. It reflects how memory actually works — both in cognitive science and in the practical constraints of AI context windows.

## Design Decisions

### Plain Markdown, not YAML/JSON/XML

CHARACTER.md files are plain Markdown because the primary consumer is a language model reading natural language. Structured formats like YAML or JSON would add parsing overhead for the human author without improving the AI's comprehension — in fact, natural prose is what language models process best. Markdown also makes files readable by humans without tooling, version-controllable with Git, and editable in any text editor.

The trade-off: Markdown lacks a formal schema. You can't validate a CHARACTER.md file the way you validate JSON against a schema. We accept this because the format's value comes from semantic clarity, not syntactic enforcement.

### Eager/lazy loading split

The main file is designed to be small enough to fit comfortably in a context window alongside the conversation. Everything else is deferred. This means an agent doesn't pay the token cost for the character's entire backstory in every message — only when it actually needs to recall something specific.

The trade-off: the agent needs a retrieval mechanism to access the folder contents. The spec deliberately does not prescribe how this retrieval works. It could be tool calls, RAG, manual copy-paste, or a platform-specific API. This makes the format portable across runtimes, at the cost of requiring each implementation to solve retrieval independently.

### "Currently" prefix for mutable state

Mutable state in Knowledges is marked with a temporal prefix (e.g., "Currently tracking three threads"). This convention exists so that both humans and AIs can instantly distinguish stable facts from state that will change. When scanning a knowledge section, anything without "Currently" is safe to treat as permanent; anything with it should be verified or updated.

The trade-off: it's a convention, not a formal mechanism. Nothing stops someone from writing mutable state without the prefix. We rely on the author's discipline rather than tooling enforcement, in keeping with the format's preference for human readability over machine validation. Note that "Currently" is the English convention; authors writing in other languages should use the equivalent temporal marker (e.g., 「目前」 in Chinese, 「現在」 in Japanese).

### Spec prescribes semantics, not mechanisms

CHARACTER.md defines what each section *means* — Dispositions are behavioral rules, Knowledges are facts, Experiences are events. It does not define how they get updated. An AI might write its own experience logs automatically; a human author might update knowledges by hand; a game engine might push state changes through an API. The spec supports all of these equally.

The trade-off: this means the spec alone doesn't give you a working system. You need to build (or choose) the runtime layer yourself. We made this choice because the format needs to work across wildly different environments — from a Cloudflare Worker serving MCP requests to a voice actor reading notes on paper. Prescribing an update mechanism would lock the format to a specific technology stack.

## Further Reading

- [SPEC.md](SPEC.md) — the full format specification.
- Sumers, T. R., Yao, S., Narasimhan, K., & Griffiths, T. L. (2023). *Cognitive Architectures for Language Agents.* arXiv:2309.02427. [https://arxiv.org/abs/2309.02427](https://arxiv.org/abs/2309.02427)
