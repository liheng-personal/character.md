# CHARACTER.md

CHARACTER.md is the whole data required for an agent to become a character who knows you and remember things in a single file or a package. Memories, skills, instructions, knowledges… CHARACTER.md has it all. 

Getting an agent to know you is cumbersome. You need to setup instructions, find memory solutions, install skills, and maintain all of them continuously, or they will soon start to contradict themselves. Worst of all? They are all different configs and services, so you need to travel back and forth just for fixing a wrong fact.

It shouldn't be this hard. Almost all information an agent needs is feed into it at the beginning of a conversation. In other words, it's just some texts for agents to read. And that's what CHARACTER.md is: all texts an agent needs, packaged in one file, loaded at the beginning of a conversation.

However, CHARACTER.md is not organized by functionalities like skills or preferences or memories. Instead, CHARACTER.md only sees semantic differences — it puts texts by three semantic categories of sentences:

1. **Dispositions**: Rules and commands. They usually come in the form of condition-instruction pairs — “When X, do Y.” Sometimes the condition can be dropped to make the disposition activated all the time — “Don’t do Y”. Dispositions include top-down rules like “don’t make up false facts,” reactions like “When being scolded, try to change the subject,” and behaviors like “When learn something valuable, save it to Knowledges.”
2. **Knowledges**: Facts and states. They are declarative statements: “X is Y.” They can also be interrogation-declaration pairs: “Is X Y? Actually it’s not that simple…” Knowledges include permanent facts and variable states. The sentence “Currently, the user has three unfinished projects” implies the statement might change frequently.
3. **Experiences**: Logs and stories. They are narratives: “X happened at 2026-06-21” or “After Y, the user decided to do Z”. They can be anchored to timelines or cause-effect chains. The past conversations are one kind of experiences. Experiences answer the question “how it has come to this?”

```
CHARACTER.md
├── Dispositions      (rules — conditional-instruction pairs)
├── Knowledges        (facts — interrogation-statement pairs)
└── Experiences       (events — narratives)
```

With this semantic structure, CHARACTER.md captures all information for agents to become characters with memories, knowledges, skills, and follow your instructions, in just one file or package.

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

**With [Morgan's CHARACTER.md](examples/agent/CHARACTER.md):**

> **You:** What should I focus on this week?
>
> **Morgan:** Three things, in order of urgency:
>
> 1. **Rekon pricing memo.** You said you'd send the three-tier recommendation by Friday. Decision's made — memo isn't drafted yet.
> 2. **Mosaic Health prep.** Monday 10am call with Rina. Expect follow-ups on the insurance input step.
> 3. **Petalink can wait.** MVP scope just locked down. No action unless Derek pings.

Morgan’s CHARACTER.md is just one file which you can bring anywhere with you, but it does more than many existing tools. The semantic-oriented structure not only makes it more comprehensive, but also make agents understand it more deeply. It might seem lacking in tooling (it’s just some texts), but it’s rich in meaning.

Furthermore, when everything is in one package, it becomes more portable and manipulatable. You can load it conditionally and dynamically. You can bring it across not only different models but different harnesses. You can even **summon multiple characters** and make them discuss topics from different viewpoints **in the same conversation**.

It’s not just a playbook for agents. It’s the whole character data, waiting for an AI model of embodiment. This is why it’s called CHARACTER.md: it IS the character.

## Getting Started

Load a CHARACTER.md file (try it with [Morgan](examples/agent/CHARACTER.md), a work assistant for a freelance product consultant) into a conversation with an agent and ask it something like this:

```
Please play this role.
```

If you want the character to learn and remember things (new rules, knowledges, and things that happened), make it editable for the agent. I use Craft to implement CHARACTER.md and connect my claude.ai chats to it, but a .md file on Dropbox, NotebookLM, or even an Airtable table can easily work. The point is for the agent to update CHARACTER.md on their own.

## Tooling Structure

```
CHARACTER.md (text content) <-> Connector <-> AI Harness.
```

The connector varies from storage providers like Dropbox and filesystem MCP server to built-in tools (NotebookLM) to tailor-made MCP server. The implementation is up for the implementor to design themselves.

## Applications

**A multi-agent team**: You can have a branding agent, a product agent, a legal agent, and more domain expert agents managing different parts of your company data for you, all in one AI harness account. Their CHARACTER.md docs act as data silos, so the agents won’t contaminate each other. Of course, you can also have them provide their opinions and discuss by themselves in the same conversation.

**A customer support agent that learns from each of its customers**: Utilize a key-value store or other structured datastore to implement CHARACTER.md, so the agent knows where to put and retrieve facts and events from for a specific customer.

**An anthropomorphized dataset**: A piece of legislation is boring to read, but what if it has a personality? CHARACTER.md can be built on its histories, guidelines, and facts.

**A game character that grows**: Define starting traits and knowledge, then let experiences fill in as players interact. The format already separates what changes from what doesn't.

**An interactive novel character**: Extract a CHARACTER.md from any novel and make an agent enact it. Perfect for book publishers and novelist to showcase how their characters feels like in person — they can even remember who the readers are.

## The Underlying Rationale

The three sentence types map to the procedural / semantic / episodic memory model from cognitive science, specifically the [CoALA framework](https://arxiv.org/abs/2309.02427). We adopted it to a more humanistic terminology of disposition, knowledge, and experience, to reflect the broader applications of CHARACTER.md. For the full design rationale, see [RATIONALE.md](RATIONALE.md).

## License

This specification is released under [CC BY 4.0](LICENSE).

## Credits

CHARACTER.md is created and maintained by [Li-Heng Hsu](https://lihenghsu.com) of [Narrativesaw](https://narrativesaw.com).

---
Annotations: 0,6975 SHA-256 f69d73c48134566a3196  
&Claude <claude.ai>: 556,2 2132,112 2258,9 2273,25 2304,19 2518,348 2867,21 2890,27 2918,425 4196 4273,53 5102,23 5126 5925,32 5958,149 6380,177 6700,192 6973,2  
@Li-Heng Hsu <lihenghsu.com>: 0,556 558,1574 2244,14 2267,6 2298,6 2323,195 2866 2888,2 2917 3343,853 4197,76 4326,776 5125 5127,798 5957 6107,273 6557,143 6892,81  
...
