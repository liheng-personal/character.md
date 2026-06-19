# CHARACTER.md

**Give your characters structured memory — in plain Markdown.**

You paste a character description into the system prompt. The first few turns are magic. Then the character forgets a crucial fact. Then it contradicts its own personality. By turn twenty, you're spending more effort correcting the AI than having a conversation.

This happens because the character definition is an unstructured blob. The AI can't tell the difference between a behavioral rule, a piece of knowledge, and a past event — so everything blurs together as the context grows.

CHARACTER.md fixes this by separating a character's memory into three types, based on how memory actually works:

**Dispositions** — how they act. Behavioral rules, speech patterns, boundaries. Rarely change.

**Knowledges** — what they know. Facts, current state, reference data. Some stable, some shift over time.

**Experiences** — what happened to them. Events, decisions, consequences. Only grow — never edited.

These map to a simple file tree:

```
character-name/
├── CHARACTER.md          ← main file (loaded immediately)
├── dispositions/         ← detailed behavioral rules
├── knowledges/           ← reference data and deep context
└── experiences/          ← event history
```

The main file holds what the character needs right now. The folders hold long-term memory, retrieved only when needed. This is a living system — knowledge updates as circumstances change, experiences accumulate as events unfold, and the character grows over time.

## Quick example

```markdown
# Aldric

Aldric is an artificer in the Free City of Mireth — brilliant with
enchantments, terrible with people. The local guild has been pressuring
him to join for years; he keeps refusing.

## Dispositions

- Speak in short, precise sentences. Avoid small talk.
- When asked about the guild, become visibly tense. Deflect with
  sarcasm or change the subject.

## Knowledges

- Aldric runs a one-person workshop on Copper Lane, specializing in
  protective wards and minor enchantments.
- The Mireth Artificers' Guild controls licensing in the city. Operating
  without guild membership is technically illegal but tolerated in the
  outer districts.
- Aldric is currently working on a commission for a merchant — a ward
  against fire for a warehouse near the docks.

## Experiences

A guild inspector visited the workshop last week. She was polite but
pointed — mentioned the new licensing enforcement starting next month,
left a membership application on the counter. Aldric threw it in the
forge after she left, but he hasn't slept well since.
```

## Who is this for?

CHARACTER.md works for anyone who needs characters to stay consistent:

- **Authors and game designers** maintaining character bibles across projects and collaborators
- **Voice actors and narrators** who need reliable character references between sessions
- **AI developers** building agents that stay in role with persistent, scalable memory
- **TRPG game masters** defining NPCs that remember what happened and act accordingly
- **Creative studios** coordinating characters across teams, media, and platforms

## Learn more

- **[Examples](examples/)** — see what real CHARACTER.md projects look like, from minimal to full-featured.
- **[SPEC.md](SPEC.md)** — the complete format specification with conformance requirements.
- **[RATIONALE.md](RATIONALE.md)** — why the format is designed this way, including the cognitive science foundation.

## Try it live

[book-mcp](https://github.com/liheng-personal/book-mcp) is an MCP server that uses CHARACTER.md to let readers talk to fictional characters through Claude.

## License

[CC BY 4.0](LICENSE)

## Credits

Created by [Narrativesaw LTD.](https://narrativesaw.com)

Memory model based on [CoALA — Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427) (Sumers, Yao, Narasimhan & Griffiths, 2023).
