---
layout: post
title: "Emacs and AI"
date: 2026-01-14
---

I've been running a gptel + gptel-agent setup in Emacs for a while now, and it's fundamentally changed how I work. Here's why I think gptel-agent is the best way to do agentic AI workflows if you're already an Emacs user.

## What is GPTel?

[gptel](https://github.com/karthink/gptel) is a simple, no-frills LLM client for Emacs. It supports multiple backends (Anthropic, OpenAI, Ollama, etc.) and lets you chat with LLMs directly from any buffer. The killer feature is that it's designed around Emacs's strengths: buffers, regions, and the minibuffer.

But the real magic happens when you add [gptel-agent](https://github.com/karthink/gptel/wiki/Agents) on top.

## The Agentic Setup

My current configuration runs Claude Opus 4.5 as the default model with a multi-agent architecture:

| Agent            | Role                             |
|------------------|----------------------------------|
| **gptel-agent**  | Main orchestrator with all tools |
| **researcher**   | Web & codebase research          |
| **introspector** | Elisp/Emacs exploration          |
| **gptel-plan**   | Read-only planning               |
| **executor**     | Autonomous file editing          |

Each agent has access to different tool sets. The main agent can delegate to specialized sub-agents based on the task at hand.

## The Tool System

This is where gptel-agent shines. The AI has access to 31 tools spanning:

- **File Operations**: `Read`, `Write`, `Edit`, `Insert`, `Glob`, `Grep`, `Mkdir`
- **Execution**: `Bash`, `Eval` (live elisp evaluation!)
- **Web Tools**: `WebSearch` (via eww/DuckDuckGo), `WebFetch`, `YouTube`
- **Introspection**: 16 tools for exploring Emacs internals
- **Task Management**: `TodoWrite` for tracking multi-step work
- **Delegation**: `Agent` for spawning sub-agents

The introspection tools are particularly powerful. The AI can browse function/variable completions, read source code, access Info manuals, and evaluate elisp expressions. It can answer questions about *my actual Emacs configuration*, not just generic documentation.

## Why This Beats Other Approaches

### 1. Deep Emacs Integration

Most AI coding tools treat your editor as a dumb text container. gptel-agent treats Emacs as what it is: a Lisp machine with decades of accumulated functionality.

The AI can:
- Navigate to function definitions with `xref`
- Search my codebase with proper regex via `Grep`
- Check if a package is loaded before suggesting code that uses it
- Access the same Info manuals I use

### 2. Sophisticated Delegation

The system prompt includes a detailed decision tree for when to use different agents:

- Open-ended research? → Delegate to `researcher`
- Understanding elisp APIs? → Delegate to `introspector`
- Multi-file refactoring? → Delegate to `executor`
- Simple single-file edit? → Handle inline

This keeps the main context clean while handling complex tasks autonomously.

### 3. No External Dependencies

Web search uses eww with DuckDuckGo. No API keys, no external services beyond the LLM itself. Everything runs locally in Emacs.

### 4. Preview & Confirmation

Destructive operations like file edits, bash commands, and elisp evaluation have preview functions. The AI proposes changes, I see a diff, and I approve or reject. No YOLO mode required (unless I want it).

## A Typical Workflow

Here's what a real session looks like:

1. I describe a task in a markdown buffer
2. Hit `C-c RET` to send
3. gptel-agent breaks it into todos
4. For research tasks, it spawns a `researcher` agent
5. For code changes, it either edits inline or spawns an `executor`
6. I get previews for any file modifications
7. Results stream back with syntax highlighting

The todo tracking is surprisingly useful. For complex tasks, I can see exactly where the AI is in the process and what's left.

## The System Prompt

The secret sauce is in the system prompt. Mine includes:

- **Tool hierarchy**: Always use `Grep` not `grep`, `Glob` not `find`
- **Delegation triggers**: Specific patterns that should spawn sub-agents
- **Response tone**: "Terse", "avoid flattery", "prioritize accuracy over agreement"
- **Critical thinking requirements**: Challenge my assumptions constructively

This eliminates the sycophantic assistant behavior and produces responses that actually help.

## Getting Started

If you want to try this:

1. Install gptel via straight.el or your package manager
2. Set up an Anthropic or OpenAI API key
3. Load the gptel-agent preset

The [gptel wiki](https://github.com/karthink/gptel/wiki) has extensive documentation. Start with the basic chat functionality, then graduate to agents when you're ready for more autonomy.

## Conclusion

gptel-agent turns Emacs into something closer to a pair programmer than a chat interface. The multi-agent architecture, deep Emacs integration, and sophisticated tooling make it possible to tackle complex coding tasks without leaving my editor.

The AI isn't taking my jerb—it's making me better at it.
