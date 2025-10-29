---
layout: post
title: "Vibing Twisp Tutorials"
date: 2025-10-29
---

Like everyone else in tech, I get a little twitchy when the South Park crew chants "They're taking our jerbs!" So let's get that out of our system real quick:

<iframe width="560" height="315" src="https://www.youtube.com/embed/APo2p4-WXsc" title="South Park - They're Taking Our Jobs!" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Okay, now that the mantra is stuck in your head again, let's talk about how AI actually helped me ship something useful today.

## Setting the Stage

We have a customer who wants to use our gRPC API. We already had folks humming along in Go and TypeScript, but this crew needed Kotlin. Our official docs lean heavily on GraphQL, and while they're fantastic, they don't walk through the streaming gRPC flows. Most engineers spelunk through the protobufs and figure it out, but I wanted a smoother on-ramp.

We do have an [excellent tutorial](https://www.twisp.com/docs/tutorials/twisp-101) and we publish our [gRPC API on buf.build](https://buf.build/twisp/api). That was enough of a breadcrumb trail to craft a prompt:

```markdown
# Twisp 101 GRPC

Twisp documents their basic tutorial using their graphql api:

1. https://www.twisp.com/docs/tutorials/twisp-101/step-1
2. https://www.twisp.com/docs/tutorials/twisp-101/step-2
3. https://www.twisp.com/docs/tutorials/twisp-101/step-3
4. https://www.twisp.com/docs/tutorials/twisp-101/step-4

In this repo let's create the streaming GRPC version. I've got a start in main.go build a complete version using the graphql examples from the tutorial steps.

Aferward write a README.md for it... Be sure to include the docker pre-requisites at:

https://www.twisp.com/docs/infrastructure/local-environment
```

## Letting the AI Drive

The `main.go` in question was a bare-bones create-journal-and-account flow over our streaming gRPC API. I pointed [OpenAI Codex](https://openai.com/codex/) at it with the `--yolo` flag and let it cook. About 20 minutes later it produced [a nice Go version](https://github.com/twisp/grpc101-go) of our tutorial. A few human polish passes and it was ready for daylight. Then I nudged it to try the Kotlin version:

```markdown
Can you implement the equivalent of https://github.com/twisp/grpc101-go in kotlin, using the appropriate buf.build sdk?
```

It obliged with a [Kotlin Version](https://github.com/twisp/grpc101-kotlin). Not perfect on the first go, but enough momentum that cleanup felt more like editing a junior engineer's pull request than starting from zero.

## Where We Landed

With an hour of prompting, I ended up with two sample apps that show off our gRPC feature set in both Go and Kotlin. The README callouts, Docker prerequisites, and streaming flows are all lifted straight from our canonical GraphQL tutorial, so new customers can hop between paradigms without losing context.

Should we still worry about our jerbs? Maybe. But if the robots keep helping me ship enablement material this quickly, I'm happy to keep vibing right alongside them.
