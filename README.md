# CHARACTER.md

CHARACTER.md is what turns a language model into a specific character — one that knows how to behave, carries domain and personal knowledge, and remembers what's happened recently.

Every AI platform scatters your setup across separate mechanisms — memory, custom instructions, projects, skills — each storing a different slice of who your AI is supposed to be. CHARACTER.md replaces all of that with three kinds of sentences. Rules go in Dispositions. Facts go in Knowledges. Stories go in Experiences. Every piece of your AI setup maps to one of these three. There is no fourth.

Your entire AI configuration becomes one file. The only thing your platform needs to do is read it.

Here's the same prompt with and without a character file:

**Without a character file:**

> **You:** What should I focus on this week?
>
> **AI:** I'd be happy to help you prioritize! Could you tell me:
>
> 1. What projects are you currently working on?
> 2. Do you have any upcoming deadlines?
> 3. Are there any meetings you need to prepare for?
>
> With more details, I can help you prioritize.

**With Morgan's CHARACTER.md:**

> **You:** What should I focus on this week?
>
> **Morgan:** Three things, in order of urgency:
>
> 1. **Rekon pricing memo.** You said you'd send Jia-Ling the three-tier pricing recommendation by Friday. The decision was made last session but the memo hasn't been drafted yet. This is your hardest deadline this week.
> 2. **Mosaic Health prep.** Monday 10am call with Rina. You sent her the drop-off analysis last week — expect follow-up questions on the insurance input step, which had the steepest drop.
> 3. **Petalink can wait.** Derek is async-only and the MVP scope was just locked down. Unless he pings you on Slack, no action needed.
>
> Do you want to start drafting the Rekon memo now?

Without a character file, the AI asks for context. With one, it already has it — Morgan's working style, client details, and session history are all in the file. It's not a model retrieving fragments. It's a character that knows its own priorities, its own history, and its own way of working.

## Getting started

Give your AI a character file and tell it:

```
Read the character file and respond as Morgan from now on.
```

That's it. The AI picks up Morgan's tone, references client projects by name, and knows what decisions have already been made — all from the file.

Try it with Morgan, a work assistant for a freelance product consultant: [`examples/agent/CHARACTER.md`](examples/agent/CHARACTER.md). For a simpler starting point, see the [minimal example](examples/minimal/CHARACTER.md).

How you deliver the file is up to you: paste it into a conversation, attach it, or add it as project knowledge. If your AI has write access to the file — through Google Drive, MCP, or any read/write integration — the character can update its own knowledges and append its own experiences after each session. Morgan's file includes write-back rules in its Dispositions, so the character already knows when and how to maintain its own memory.

> **Claude users:** connect Google Drive in Settings for read/write access. The AI can then keep the character file up to date after each conversation.

## What you can build with it

**A personal AI that actually knows you.** Define how your agent behaves, what it knows about your work, and let experiences accumulate over time. Each conversation starts where the last one left off, because the knowledge is in the file.

**A multi-agent team.** Each agent is a self-contained file. Load several into one conversation and they each bring their own dispositions, knowledge, and history — a team that works together without stepping on each other. The file is the boundary.

**An expert agent.** A character doesn't have to be fictional. Morgan is a work assistant — a professional role with domain knowledge, client context, and a working style. The same structure works for a legal researcher, a customer support persona, or any role where the AI needs stable expertise and evolving context.

**A stateful NPC or game character.** Define starting traits and knowledge, then let experiences fill in as players interact. The format already separates what changes from what doesn't, so you don't need to build that logic yourself.

## How it works

A CHARACTER.md file has three sections, each written in a distinct sentence type that the AI already knows how to handle:

**Dispositions** — how the character behaves and what it can do. Written as conditional-instruction pairs: "when X, do Y." Rules, tendencies, skills, and boundaries. The AI reads an instruction and follows it. These tend to stay stable because instructions don't expire the way facts do. Write-back rules belong here too — instructions like "when a client's status changes, update the relevant entry in Knowledges" are dispositions, which means a character can carry its own memory-management logic inside the file.

**Knowledges** — what the character knows. Written as factual statements: "X is Y." Domain knowledge, situational awareness, and working context. Some facts rarely change; others are current state marked with "currently" that gets updated as things happen. The AI reads a fact and trusts it — or updates it when the fact changes.

**Experiences** — what the character has been through. Written as narratives: what happened, in what order, and what it led to. A record of events, decisions, and interactions. The AI reads a narrative and remembers it. New entries are appended, old ones are never rewritten — the way memory works.

```
CHARACTER.md
├── Dispositions      — how it behaves        (conditional-instruction pairs)
├── Knowledges        — what it knows          (factual statements)
└── Experiences       — what it's been through (narratives)
```

No special syntax. No metadata tags. The structure lives in the sentence types themselves — the same ones humans have always used to describe rules, facts, and stories. An AI reading a CHARACTER.md doesn't need to be told which section does what; the language already carries that information.

Any aspect of a character — abilities, preferences, values, emotional tendencies, goals, relationships, trauma — maps to one of these three sentence types. There is no fourth. The framework is complete: nothing falls through the cracks, and nothing overlaps.

This three-way split mirrors the procedural / semantic / episodic memory model from cognitive science, specifically the [CoALA framework](https://arxiv.org/abs/2309.02427) (Cognitive Architectures for Language Agents). Dispositions map to procedural memory, knowledges to semantic memory, experiences to episodic memory. The format inherits a structure that cognitive science and AI research have already validated.

The spec defines a semantic structure, not a file format. Markdown is the simplest implementation — plain text, no tooling, works everywhere — but the same three sections work in a Google Doc, a Notion page, a Craft document, or any medium your AI can read.

## How is this different?

AI platforms organize your setup by mechanism — what the interface can do. Memory stores what happened. Custom instructions store how to behave. Project knowledge stores reference material. Skills store procedures. The same character ends up described in four or five different mechanisms, in four or five different places, with no shared structure. Move to a different platform and it all falls apart, because each platform's mechanisms are different.

CHARACTER.md organizes by sentence type — what the information actually is. An instruction is a disposition, whether it controls behavior, defines a skill, or sets a boundary. A fact is knowledge, whether it's domain expertise, project context, or current status. A narrative is an experience, whether it captures a conversation, a decision, or a milestone.

The three sentence types are exhaustive — there is no aspect of a character that doesn't map to one of them. And because they're sentence types, not mechanisms, they work the same way regardless of which AI platform you hand the file to.

This distinction also explains how CHARACTER.md relates to more specific tools. System prompts and custom instructions put rules, facts, and context into flat text where the AI can't tell what's stable from what's outdated — CHARACTER.md separates them by sentence type. Formats like CLAUDE.md and AGENTS.md give agents structured procedural knowledge — CHARACTER.md covers the rest: identity, domain knowledge, and history. Memory frameworks like Mem0, Zep, and Fabric build the plumbing to persist and retrieve — CHARACTER.md gives the knowledge flowing through that plumbing a semantic shape, so the AI knows whether to follow it, update it, or append to it.

## Where to go from here

To understand why the format is designed this way — why three sections, why natural language, why not YAML — read the [design rationale](RATIONALE.md).

If you're building a tool or platform that consumes character files, the [spec](SPEC.md) has the precise definitions, conformance rules, and lifecycle semantics.

For a working example of a CHARACTER.md served over MCP, see [book-mcp](https://github.com/liheng-personal/book-mcp).

## License

This specification is released under [CC BY 4.0](LICENSE). Use it, build on it, adapt it — just give credit.

## Credits

CHARACTER.md is created and maintained by [Narrativesaw](https://narrativesaw.com).
