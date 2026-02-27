---
layout: post
title: "Should I Stop the Train? Systems Engineering and the Future of Writing Code"
date: 2026-02-27
---

Before I was a web guy, I was a train guy. Or more accurately, I was a software guy who happened to work at a train company. This distinction mattered more than you might think.

I spent nearly 2 years at Wabtec working on Positive Train Control, or PTC. If you've never heard of PTC, congratulations on not spending your twenties debugging C code on a real-time operating system in a windowless office in the Cedar Rapids Iowa. But the work was fascinating in a way that I didn't fully appreciate until much later, when I watched the entire software industry start stumbling toward the same conclusions that railroad systems engineers reached decades ago.

## Should I Stop the Train?

At its core, PTC is beautifully simple. You have a computer on a train, and it's running a loop. On every pass through that loop, it's asking itself one question: should I stop the train?

![trolly]({{ site.url }}/assets/images/trolly.jpg)
*The ultimate boolean...if the brakes fail.*

That's it. That's the whole product. One boolean decision, made over and over again, every second.

Of course, answering that question requires knowing a few things:

1. What is the current speed of the train?
2. What is the current braking distance of the train?
3. At my current speed, when will I hit a speed limit where braking will be required?
4. What speed limits is the train currently operating under?
5. What are the upcoming speed limits?
6. What [operating rules](https://www.ecfr.gov/current/title-49/subtitle-B/chapter-II) govern my current state?
7. What railroad policies affect my current state?

Seven inputs to answer one question. The physics of a multi-thousand-ton object moving at speed, the sensor data streaming in from GPS and inertial navigation and air brake pressure gauges, the federal regulations that define what "safe" means, and the railroad-specific operating rules layered on top. All of it funneling into a single bit: stop or don't stop.

Simple question. Not a simple system.

## It's Never Simple

Here's where it gets fun. Even if you nail the physics and the regulations and the sensor fusion, you've still got problems that no amount of math will solve.

**Problem one: air pressure.** Trains use air brakes. When the system commands a brake application, it bleeds off air pressure in the brake line. That's fine, that's how brakes work. But if the system commands too many brake applications in quick succession, you can bleed off so much air pressure that you lose your pneumatic brakes entirely. Now you've got a multi-thousand-ton object rolling down the track with nothing but mechanical brakes. The safety system designed to prevent disasters has just created a new kind of disaster.

**Problem two: humans.** The system is supposed to enforce speed limits. Crews are supposed to operate within those limits. But what actually happens is that some crews figure out exactly where the enforcement boundaries are and treat them as targets. The system says "if you don't slow down in 500 feet, I'm stopping you." The crew hears "I can go this fast for another 499 feet." My brother was a conductor on the Union Pacific, and he saw this constantly. Operators gaming the safety system, pushing right up to the edge of an enforcement action because they knew exactly where the line was.

**Problem three: trust.** False positives erode trust. If the system slams the brakes when nothing is wrong, the crew starts wanting to turn it off. And in the early days of PTC, there were plenty of false positives. Bad GPS fixes, stale track data, communication dropouts. Every false enforcement made the next real enforcement feel less credible.

Physics, humans, computers, rules. They all mix together, and hopefully the result is that potentially dangerous situations become less so. But "hopefully" is doing a lot of work in that sentence.

## The Feudal Order

Here's something that surprised me when I first started working in a regulated industry: the software developers were the lowest rung on the engineering ladder.

In web development, software engineers are the main characters. We write the code, we make the architecture decisions, we have opinions about tabs versus spaces that we're willing to die on. But in regulated industries like rail, aviation, and especially defense, the Systems Engineer is king.

Systems engineers define the requirements. They write the specifications. They trace requirements down through multiple levels of decomposition until each one maps to a testable acceptance criterion. They work with test engineers to verify that the system does what it was designed to do. And then the software engineers implement whatever the systems engineers have specified. You're not designing anything. You're translating requirements into code.

It's a feudal hierarchy, and software is the serf.

This wasn't an accident, and it wasn't just organizational inertia. There was a real intellectual tradition behind it. Grady Booch, who was chief scientist at Rational Software, helped create the Unified Modeling Language (UML) with exactly this vision in mind. The idea was that systems requirements themselves would eventually be so rigorously defined that they could be compiled into code, implemented automatically without human programmers in the loop. Rational built the tooling, the methodologies, the certification frameworks. The entire premise was that requirements were the hard part, and code generation was a solved problem waiting to happen.

Systems was always going to win. As a computer nerd, I enjoyed the low-level details. How does the device communicate with the computer? What does the interrupt handler look like? But the important decisions were happening a few abstraction levels up. 

![Always has been]({{ site.url }}/assets/images/Always_has_been.jpg)
*Always has been.*

## What's Under the Hood

Before I get to the punchline, I want to show you what one of these systems actually looks like. Not because it's the most elegant architecture ever conceived, but because I think there's something instructive about seeing the concrete reality behind all this theory about systems engineering.

The PTC system was built in C on QNX. QNX is a real-time operating system (RTOS) that gives processes guaranteed time slices in its scheduler. When your software is deciding whether to stop a train, "the garbage collector paused for 200ms" is not an acceptable excuse.

Processes communicated through messages on a UDP bus via a pub-sub mechanism. Each process was essentially a service that subscribed to the messages it cared about and published its own state for other services to consume. A service-oriented architecture, years before anyone was putting that phrase on conference slides.

Here's what the service landscape looked like:

| Service      | Purpose                                                                  | Subscribers            |
|--------------|--------------------------------------------------------------------------|------------------------|
| timer        | sending 1hz task messages                                                | *                      |
| switch mgr   | broadcast state of switches                                              | map, target            |
| signal mgr   | broadcast state of signals                                               | map, target            |
| location     | broadcast GPS coordinates                                                | map                    |
| map          | broadcast current resolved map w/track data                              | switch, signal, target |
| target mgr   | manages current targets                                                  | enforcement            |
| enforcement  | determines if we should warn or apply brakes                             | brake mgr              |
| brake mgr    | apply the brakes or not                                                  |                        |
| display mgr  | manages rendering of maps, targets etc                                   |                        |
| dispatch mgr | manages dispatched speed limits/targets etc                              | target mgr             |
| train mgr    | manages metadata about the train itself: length, weight, locomotives etc | *                      |

And each of these services may or may not have sensor hardware feeding it real-world data:

| Service    | Sensors                                                                 |
|------------|-------------------------------------------------------------------------|
| switch mgr | radio interrogate switch, camera (visual switch position identification) |
| signal mgr | radio interrogate signal                                                |
| location   | GPS, inertial navigation                                                |
| brake mgr  | air brake pressure sensor, current brake state                          |
| train mgr  | speedometer, fuel sensor                                                |

And here's how it all fits together:

```
                         ┌─────────────────────────────────────┐
                         │           SENSORS / INPUTS          │
                         └─────────────────────────────────────┘
                           │         │           │      │     │
                         GPS/INS  Radio        Radio   Air   Speed
                           │      Camera         │     Brake Fuel
                           │         │           │      │     │
┌──────────┐          ┌────▼───┐ ┌───▼────┐ ┌────▼──┐ ┌─▼─────▼─┐
│  timer   │──1hz──►  │location│ │switch  │ │signal │ │ train   │
│  (1hz)   │  tick    │  mgr   │ │  mgr   │ │  mgr  │ │  mgr    │
└──────────┘          └───┬────┘ └──┬──┬──┘ └─┬──┬──┘ └────┬────┘
     │                    │         │  │      │  │         │
     │                    │ GPS     │  │      │  │     weight/length
     │                    ▼         │  │      │  │         │
     │               ┌─────────┐◄───┘  │  ┌───┘  │         │
     │               │   map   │       │  │      │         │
     │               │  mgr    │───────┼──┼──────┘         │
     │               └────┬────┘       │  │                │
     │                    │ track      │  │                │
     │                    │ data       │  │                │
     │                    ▼            ▼  ▼                │
     │    ┌──────────┐  ┌──────────────────┐               │
     │    │ dispatch │  │   target mgr     │◄──────────────┘
     │    │   mgr    │─►│  (manages speed  │
     │    └──────────┘  │   targets)       │
     │                  └────────┬─────────┘
     │                           │ targets
     │                           ▼
     │                  ┌──────────────────┐
     │                  │   enforcement    │
     │                  │                  │
     │                  │  "should I stop  │
     │                  │   the train?"    │
     │                  └────────┬─────────┘
     │                           │ warn / brake
     │                           ▼
     │                  ┌──────────────────┐
     │                  │   brake mgr      │──────► BRAKES
     │                  └──────────────────┘
     │
     │                  ┌──────────────────┐
     └─────────────────►│  display mgr     │──────► CREW DISPLAY
                        └──────────────────┘

         ═══════════════════════════════════
              All messages flow over
              TCP pub-sub bus (QNX/C)
         ═══════════════════════════════════
```

Eleven services. A handful of sensors. One question, asked forever: should I stop the train?

Look at the flow. Sensors feed raw data into managers. Managers resolve that data into meaningful state. State flows into the target manager, which figures out what speed limits and restrictions apply right now. The enforcement service takes those targets and the current train state and makes the call. And the brake manager does the physical thing.

It's a pipeline. Messy real-world inputs at the top, a single binary decision at the bottom. Every layer is doing one thing: reducing uncertainty. By the time you get to the enforcement service, the whole chaotic world of GPS satellites and radio signals and switch positions and federal regulations has been compressed into a question simple enough that a computer can answer it reliably, a few times per second, on hardware that was considered underpowered in 2008.

## The Prophecy Comes True

So here's the thing I keep thinking about.

Grady Booch and the Rational crew were right about the destination. They were just wrong about the vehicle. UML didn't compile into working code. Model-driven architecture remained a conference talk, not a shipping product. The tooling was too rigid, the models too brittle, the gap between a box-and-arrow diagram and working software too vast for automated translation.

But LLMs are closing that gap in a way that nobody predicted. Not by compiling formal specifications, but by interpreting informal ones. You describe what you want in plain language, with enough structure and context, and the model produces working code. It's not the clean compile-from-spec vision that Booch imagined. It's messier, more probabilistic, more like explaining things to a very fast junior engineer than feeding punch cards into a compiler. But the result is converging on the same place: systems-level thinking becomes the bottleneck, and code generation becomes the commodity.

I'm watching this happen in real time in web product development, which is about as far from regulated rail systems as you can get. Teams that used to be organized around who could write the best React components are starting to reorganize around who can design the best systems. Who can decompose a problem into the right services? Who can define the interfaces clearly enough that an LLM can implement them? Who can think about failure modes and edge cases before they become production incidents?

The future software engineer looks a lot like a systems engineer who can iterate fast. Someone who's encountered enough real systems to have good intuitions about decomposition, who can document their designs clearly enough for automatic implementation, and who can evaluate whether the generated output actually satisfies the requirements. Not someone who's optimizing their vim macros for typing speed.

The feudal order is reasserting itself, just in a different kingdom.

## So What Now?

Here's the question I can't answer, and it's the one that keeps me up at night.

If LLMs make code generation fast, the systems engineers win. The people who can think clearly about requirements, decomposition, and interfaces become the most valuable engineers. That's the optimistic read, and I mostly believe it.

But the same LLMs that generate code can also accelerate systems design. They can help you decompose problems, identify edge cases, draft specifications, suggest architectures. If the tool that commoditizes implementation also commoditizes design, then what's the strata? What layer of the stack is still meaningfully human?

In the PTC system, we had layers of assurance. Federal regulations. Systems requirements. Test procedures. Independent verification and validation. Multiple organizations with different incentives all checking each other's work. The reason a train safety system works isn't just that it was well-designed. It's that the process for designing it was itself well-designed, with enough friction and oversight to catch the mistakes that any single engineer would inevitably make.

When we accelerate everything, what provides that friction? If one person with an LLM can do the work that used to require a systems engineer, three software developers, and a test team, who's checking the work? The LLM? Another LLM? Do we just trust the vibes?

I don't have an answer. But I think the question matters, and I think the train people figured out something important that the rest of us are still learning: the hard part was never writing the code. The hard part was knowing what the code should do, and being sure enough about it to bet lives on the answer.
