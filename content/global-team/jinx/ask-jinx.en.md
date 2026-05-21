---
title: "Asking Jinx a question"
menuTitle: "Asking Jinx"
weight: 30
---

<img src="/img/jinx/support.svg" alt="Jinx the witch's cat, lending a paw" width="140" align="right" style="margin: 0 0 1rem 1.5rem;">

_Runs in: **Cloudflare Worker** ([`worker/src/slack-events.js`](https://github.com/rladies/jinx/blob/main/worker/src/slack-events.js), [`worker/src/rag.js`](https://github.com/rladies/jinx/blob/main/worker/src/rag.js)) + **Cloudflare Vectorize** + **Workers AI**. No GitHub Actions, no R package._

When you DM Jinx in Slack, mention them in a channel, or use the Slack Assistant panel, your message is treated as a question -- not a slash command.
There is no `/jinx` prefix, no command to remember; just ask.

Jinx searches a Cloudflare Vectorize index of RLadies+ content and replies with a grounded answer plus links to the sources it used.
React to the answer with 👍 / 👎 / ❤️ and we collect that signal to track which answers are useful (see `/jinx feedback` on the [Commands]({{< relref "commands" >}}) page).

## How a question becomes an answer

```mermaid
sequenceDiagram
    autonumber
    participant U as Person
    participant S as Slack
    participant W as Cloudflare Worker
    participant E as Workers AI<br>(BGE embeddings)
    participant V as Vectorize<br>(rladies-content)
    participant L as Workers AI<br>(Llama-3.1)
    U->>S: DM / @-mention / Assistant message
    S->>W: event_callback
    W->>W: intent check<br>(coding question? decline)
    W->>E: embed(question)
    E-->>W: vector
    W->>V: top-k similarity search
    V-->>W: candidate chunks + metadata
    W->>W: rerank<br>(source weight × recency × staleness × audience)
    W->>L: prompt + top 5 chunks
    L-->>W: grounded answer
    W->>W: repair links<br>(strip URLs not in sources)
    W->>S: chat.postMessage with citations
    S-->>U: answer + 👍/👎/❤️ reactions
```

A few details worth knowing:

- **Coding questions are politely declined.** "Debug this regex" or "why is my ggplot empty?" gets a pointer to `#help-r`. Jinx is an organisational guide, not a coding assistant.
- **The reranker favours canonical sources.** Pages from the guide and main website outrank R package READMEs and other community content. `/global-team/` pages (maintainer-facing) are down-weighted relative to user-facing equivalents.
- **Future-dated content (upcoming events) gets a boost.** The recency factor clamps to 1.0 for future dates, so "what's coming up?" surfaces upcoming meetups above past ones.
- **Link integrity is enforced.** Any URL the model emits that does not appear in the retrieved sources is stripped (the label stays). Jinx will never invent a link.

## Welcome on join

When a person joins either RLadies+ workspace, the worker receives a `team_join` event and DMs them with a workspace-specific welcome message rendered from a markdown template in `inst/templates/`.

If the new member's email matches a pending chapter sign-up coming in from Airtable (see [Airtable invite webhook]({{< relref "airtable-invites" >}})), the welcome notes that we matched them up.

## Assistant panel suggestions

Opening the Jinx Assistant in Slack triggers an `assistant_thread_started` event; the worker sets the thread title and four suggested prompts.
Both the title and the prompts come from [`inst/config/assistant-prompts.json`](https://github.com/rladies/jinx/blob/main/inst/config/assistant-prompts.json) in the jinx repo, so they can be edited without redeploying the worker.
