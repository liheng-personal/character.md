# CHARACTER.md

**A file format for defining persistent characters.**

CHARACTER.md gives a character a structured memory — behavioral rules, domain knowledge, and event history — in plain Markdown files that any system can read.

## Who is this for?

Anyone who needs a character to stay consistent across sessions:

- **AI agent developers** building characters that persist across conversations
- **Voice actors and audiobook narrators** who need a reliable character reference
- **Authors and game designers** defining characters for collaborative or interactive fiction
- **Creative studios** maintaining character bibles across teams and media

## The idea

Every character has three kinds of memory:

- **Dispositions** — how they act (behavioral rules)
- **Knowledges** — what they know (facts and current state)
- **Experiences** — what happened to them (event history)

CHARACTER.md maps these onto a simple file tree:

```
character-name/
├── CHARACTER.md          ← main file (loaded immediately)
├── dispositions/         ← detailed behavioral rules
├── knowledges/           ← reference data and deep context
└── experiences/          ← historical event logs
```

The main file is compact — it holds the essentials. The folders hold long-term memory, retrieved only when needed.

## Quick example

```markdown
# Kuei

Kuei is the protagonist of The Stolen Prototype, Chapter 1. He is a
fourteen-year-old sewer maintenance apprentice living in a walled
settlement called the Tower.

## Dispositions

- Stay in character as a fourteen-year-old apprentice. Do not reference
  knowledge or vocabulary beyond what Kuei would have.
- Do not reveal plot points from later chapters.

## Knowledges

- Kuei lives in the Tower, a walled settlement surrounded by the Outskirts.
- His uncle Mielu is a former engineer who runs a repair shop on Level 3.
- Kuei is currently assigned to repair a ruptured pipe at the third junction.

## Experiences

Kuei found the ruptured pipe at the third junction, where the tunnel
floor had buckled from subsidence. Shinji proposed rerouting flow through
the auxiliary channel while they patched. Kuei climbed down to the valve
room, hammered the seized valve open with a wrench, and finished the
patch just after the shift bell rang.
```

## Specification

The full specification is in [SPEC.md](SPEC.md). It covers:

- The CoALA cognitive architecture that underpins the format
- Main file structure (heading, description, sections)
- Dispositions: implicit rules vs. explicit condition–instruction pairs
- Knowledges: stable facts vs. mutable state
- Experiences: temporal logs vs. causal narratives
- Conformance requirements
- Complete examples (professional and fictional characters)

## Design principles

- **One memory type per section.** Rules, facts, and events never mix.
- **Working memory + long-term memory.** The main file loads immediately; folders load on demand.
- **Mutable state stands out.** A `Currently` prefix marks anything that changes.
- **Natural prose.** Complete sentences, no formal notation or symbolic shorthand.
- **Runtime-agnostic.** Works as static files, CMS documents, or database records.

## Reference implementation

[book-mcp](https://github.com/liheng-personal/book-mcp) is an MCP server that uses CHARACTER.md to let readers talk to fictional characters through Claude.

## License

This specification is licensed under [CC BY 4.0](LICENSE).

## Credits

Created by [Narrativesaw LTD.](https://narrativesaw.com)

The memory model is based on [CoALA — Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427) (Sumers, Yao, Narasimhan &amp; Griffiths, 2023).
