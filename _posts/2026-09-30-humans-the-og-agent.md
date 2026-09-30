---
layout: post
title: "Humans: The OG Agent"
date: 2026-09-30
---

Before we gave AI agents tools, customer service agents had a pretty good one: a DM to somebody in engineering who gave a rip.

At Simple, sometimes that somebody was me.

## At Simple I Was the MCP

We had built our own card processor. Sometimes you don't build all the UX features you actually need for the real world.

Shortly after a stressful card processor migration, conversations like this started happening. Probably real, definitely representative:

```text
Customer: I got super drunk and lost my debit card.

Agent: I got you homie, deactivating the old one and shipping you a new one.

...

Customer: Whoops, I found it. Can you turn it back on?

Agent: Yeah, one sec!
```

That last bit wasn't in the console yet.

Luckily, as any customer service agent from that era can attest, we had the world's first MCP: Mike Can Python.

One of my core tools was `resurrect.py`.

```text
Agent: yo snips, customer 284fa048-bd03-11f1-b4d7-020000000000
       needs their card resurrected.

MCP: done!
```

It could raise the card from the dead by invoking the right sequence of APIs. I must have been invoked hundreds of times before that capability made it into our internal console.

The customer service agent understood what the customer needed, found the tool, and got it done. The tool just happened to be a guy with Python and a DM inbox.

## That Isn't Bank Grade

But we weren't a bank.

We were legendary for our customer service responsiveness, and it was due to having people who cared deeply about excellence, and doing right by our customers. From the frontline to the backend. That was a great culture to work in.

But this workflow wasn't bank grade:

1. Agent asks backend engineering for intervention via DM.
2. Backend engineer runs an API script with low visibility.
3. ???
4. Customer happy.

No audit trail for the workflow. No permission escalation path. Just pure customer happiness as the goal.

The missing product feature had become a human process. You knew who to ask, and they knew what to run.

## The AI Era Allows Happiness

Purely unconstrained. Simple.com style.

Add some MCPs. Toss in some OAuth via your SSO provider. Turn on your agent's agent and make stuff happen.

Customers are happy. Winning. Right?

![Four-panel Anakin and Padmé meme. Anakin says, "The AI agent replaced the customer's card." Padmé asks, "And the replacement was audited, right?" Anakin silently smirks. A worried Padmé repeats, "It was audited, right?"]({{ '/assets/images/card-replacement-audited-meme.png' | relative_url }})

Did the agent hallucinate? Is the process getting eval'd? Do we even know who or what kicked things off? Did they have permission to do that? Can we do this thousands of times a day?

We can give everyone the equivalent of a backend engineer in their DMs. We can also reproduce the gaps in that workflow at a speed I could never have managed with `resurrect.py`.

## Bringing Back Bank Grade

I want everyone in a company to have that ability to delight a customer. The feeling that you backchanneled your backend team and somebody said, "Yeah, one sec!"

I also want the system to be able to answer the questions afterward. Who asked? Who or what acted? What were they allowed to do? What changed? And I want the permission checks to happen before the action runs.

That's what I mean by bringing back "bank grade." Make that responsiveness something the system can support, thousands of times a day.

I see a few companies working on this. [Arcade.dev](https://www.arcade.dev/) is one, with its focus on authorization and auditing for agent actions.

Twisp will be another. We've been in the lab working on bringing back "bank grade," right down to supporting SOX compliance, while delighting our customers and their customers.

I can't wait to show y'all what we've been cooking.

Doing it bottoms up, as usual. That's the Twisp way.
