# Design Rationale

This document explains *why* CHARACTER.md is designed the way it is — the problem it solves, the cognitive science behind its structure, and the trade-offs behind its key design decisions.

## The Problem

Every AI platform organizes what it knows about you differently. One keeps behavioral rules in "custom instructions." Another stores facts in "memory." A third splits knowledge across "projects" and "skills." Each mechanism has its own location, format, and limitations.

This means the same piece of information — say, that you always want citations in your reports — lives in different places depending on which platform you use. Move to a new tool and you start over. Use two tools in parallel and they contradict each other. Use one tool long enough and its own mechanisms contradict each other, because nothing enforces consistency across memory, instructions, and project knowledge.

The root cause is not that these mechanisms are poorly built. It is that they classify information by *mechanism* — by where the platform happens to store it — rather than by *meaning*. A behavioral rule and a factual statement end up in the same "memory" bucket. A preference and a procedure share the same "instructions" field. When the categories don't match the content, drift is inevitable.

The same structural problem surfaces wherever characters or agents need to persist: game designers whose NPC definitions don't survive handoffs between writers and programmers, voice actors who receive character guides that blur personality with backstory, AI assistants that lose coherence across sessions because their knowledge has no internal structure.

CHARACTER.md addresses this by organizing information according to what it *means* — three semantic categories that correspond to fundamentally different kinds of sentences.

## Why Three Semantic Categories

Almost any sentence you would write to define a character falls into one of three types. They differ in what they express, how they are structured, and how they change over time.

**Dispositions** — rules that govern how the character acts. Their basic unit is the condition–instruction pair: "When X, do Y." Sometimes the condition is implicit — always active, never stated — making the instruction appear unconditional: "Never fabricate sources." Dispositions vary along two independent axes: by *activation* (implicit or explicit) and by *function* (constraints that must be followed, or tendencies that describe how the character naturally reacts). A constraint violated is an error; a tendency overridden by context is not.

**Knowledges** — facts about the world, the character, or the domain. They take the form of declarative statements ("X is Y") or question–answer pairs ("Q: What is X? → A: X is Y"). Some are stable facts that do not change. Others are mutable state — current conditions that shift as circumstances evolve. The two coexist within the same section but must be distinguishable, so a reader or system can tell which knowledge is settled and which is subject to change.

**Experiences** — events organized along a narrative axis. Unlike knowledges, which describe what is true now, experiences record what happened: decisions made, outcomes observed, context that shaped them. They may be organized temporally (by when things occurred), causally (by what led to what), or without a particular axis (simply recording that something happened). These organizations are not mutually exclusive.

These three categories are exhaustive. Try to think of a fourth kind of sentence that defines a character — a sentence that is not a rule, not a fact, and not an event. The difficulty of finding one is itself the argument for completeness. The categories do not overlap: a sentence that describes a static attribute belongs in Knowledges, while a sentence that describes a reaction to a condition belongs in Dispositions. The test is whether the sentence contains a stimulus and a response. "Prefers formal language" is a fact. "When addressed casually, responds formally" is a disposition.

When all three are mixed into a single prompt or document, the system cannot distinguish which is which. A rule gets softened by a nearby anecdote. Current state gets confused with historical state. An event gets treated as a permanent fact. Semantic separation solves this by letting each category be stored, retrieved, and updated according to its own nature.

## Cognitive Foundation

The three-way split is not arbitrary. It maps onto a well-established model in cognitive science: the distinction between **procedural memory** (knowing how), **semantic memory** (knowing what), and **episodic memory** (knowing when/where something happened).

CHARACTER.md specifically draws on **CoALA** — Cognitive Architectures for Language Agents (Sumers, Yao, Narasimhan & Griffiths, 2023), which adapts this cognitive framework to the architecture of AI agents. CoALA proposes that an effective language agent needs working memory (what is currently loaded into context) and long-term memory (persistent storage retrieved on demand), with long-term memory further divided into procedural, semantic, and episodic types.

CHARACTER.md adopts this cognitive structure but translates it into a more humanistic vocabulary — Dispositions, Knowledges, and Experiences — to reflect the specification's broader scope. CoALA describes a memory architecture for AI agents; CHARACTER.md defines the semantic structure of a character that can be performed by any system, including but not limited to AI agents. The specification defines what kinds of information a character contains and how they relate to each other. It does not prescribe how that information is stored, loaded, or updated — those decisions belong to the implementation.

## Design Decisions

### Natural language sentences, not structured data

The basic unit of CHARACTER.md is the *sentence*: the smallest semantically complete piece of natural language. A sentence is defined by semantic completeness, not punctuation — it may be a single typographic sentence or span an entire paragraph, as long as it expresses one complete idea.

This choice has consequences. Natural language is what language models process best, and it is what humans read without tooling. A character definition written in natural language sentences can be loaded into any AI system, read by a voice actor, or handed to a game designer — without parsers, schemas, or format-specific tooling. The specification gains universality by building on the most widely understood medium there is.

The trade-off: natural language lacks a formal schema. You cannot validate a CHARACTER.md definition the way you validate JSON against a schema. The specification accepts this because its value comes from semantic clarity, not syntactic enforcement.

### Medium-agnostic specification

The specification defines semantic structure — what kinds of sentences a character definition contains and how they relate to each other. It does not prescribe file formats, directory layouts, storage mechanisms, or update procedures. A conformant character definition could be a Markdown file, a Google Doc with three headings, a set of Craft subpages, a Notion database, or a row in Airtable.

This decision follows from the core diagnosis. The problem CHARACTER.md solves is that information gets scattered when classified by mechanism — by the storage format or platform feature it happens to live in. Binding the specification to a particular file format would reintroduce the same problem at a different level: the character's portability would be limited by the format rather than by the platform. A semantic specification is portable by nature, because meaning does not change when the container does.

The trade-off: implementors must decide for themselves how to realize the semantic structure in their chosen medium. The specification gives them the *what* but not the *how*. This is deliberate — the range of environments where CHARACTER.md needs to work (from Cloudflare Workers to paper notes) is too wide for any single implementation prescription to serve.

### Constraints and tendencies, not rules and personality

Dispositions are divided by semantic function into constraints (obligations and prohibitions that must be followed) and tendencies (reactive patterns that the character naturally exhibits). This replaces earlier framings that split dispositions into "external rules" versus "internal behavior patterns."

The reason: the earlier framing was based on *who defined the rule* (external authority versus internal nature), but this is not a semantic distinction. A constraint can come from within ("I never lie") just as a tendency can come from outside ("corporate training makes her default to formal language"). The semantically meaningful difference is whether violation constitutes an error. A constraint violated is a failure; a tendency overridden by context is normal.

The trade-off: this classification requires the author to make a judgment about each disposition — is this a rule the character must follow, or a pattern the character tends to exhibit? The specification does not automate this judgment. In practice, the distinction is usually obvious from context, but edge cases exist and are left to the author's discretion.

### Semantics, not mechanisms

CHARACTER.md defines what each section *means*. It does not define how sections get stored, how they get loaded into context, how they get updated, or how an implementation should split content between eager and lazy retrieval. An AI might write its own experience logs automatically. A human author might update knowledges by hand. A game engine might push state changes through an API. A voice actor might read the whole thing on paper. The specification supports all of these equally.

This principle extends further than it might first appear. The specification does not prescribe the "Currently" prefix or any other convention for distinguishing mutable state from stable facts — only that they must be distinguishable by *some* means. It does not prescribe file extensions, directory structures, or heading syntax — only that the three sections appear in order. Every requirement in the specification is a semantic requirement. If a requirement would only make sense in a particular medium, it does not belong in the specification.

The trade-off: the specification alone does not give you a working system. You need to build (or choose) a storage layer, a loading mechanism, and — if you want the character to grow — a write-back process. This means more work for the implementor, but it also means the specification can serve environments that no single implementation could anticipate.

## Further Reading

- [SPEC.md](SPEC.md) — the full specification.
- Sumers, T. R., Yao, S., Narasimhan, K., & Griffiths, T. L. (2023). *Cognitive Architectures for Language Agents.* arXiv:2309.02427. [https://arxiv.org/abs/2309.02427](https://arxiv.org/abs/2309.02427)
