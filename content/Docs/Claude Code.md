---
title: Claude Code
---

I've been using Claude Code for a few weeks now, and I'm fairly certain it's going to change how I work.

## What it is

Claude Code is Anthropic's CLI tool that lets Claude operate directly in your terminal. It can read files, write code, run commands, search your codebase, and commit changes. You talk to it, it does things. It doesn't give you code snippets to copy. It actually makes the changes.

This is different from chatting with Claude in a browser and pasting code back and forth. The context is your actual project. It sees your files, understands your structure, runs your tests.

## Why it clicks for me

The shift is from "Claude as a search engine for code" to "Claude as a collaborator that happens to type faster than me."

I can say "rewrite this article, it sounds too generic" and it reads the file, rewrites it, and saves it. I can say "commit and push" and it does. The friction between having an idea and executing it drops significantly.

It's also good at exploring unfamiliar codebases. Instead of grepping around trying to understand how something works, I can ask "how does the plugin system work here?" and get an actual answer based on the code, not documentation that might be outdated.

## Subagents and skills

This is where it gets interesting. Claude Code supports subagents and skills. These are specialized agents you can configure for specific tasks. Need to explore a codebase? There's an agent for that. Need to run tests? Another agent. Want a custom workflow for commits or PR reviews? You can build that.

It's starting to feel like having my own little IT department. Instead of one generalist assistant, I can delegate to the agent best suited for the job. "You handle the architecture research, you run the tests, you do the code review." Each one stays focused on what it's good at.

I'm still configuring and discovering what's possible here, but the potential is significant. The difference between "one AI that does everything" and "a team of specialized AIs I can coordinate" is larger than I expected.

## What I'm still figuring out

When to intervene vs. let it run. Sometimes it makes choices I wouldn't make, and I'm learning when to steer and when to just fix it after. The balance between "let it flow" and "no wait, stop" takes calibration.

Also: trusting it with destructive operations. It's careful and asks for confirmation on risky stuff, but there's still a mental adjustment to letting something else run commands in your terminal.

## The workflow shift

Before: think → research → write code → test → debug → repeat.

Now: think → describe what I want → review what it did → adjust → done.

The "research" and "write code" steps compress dramatically. I still need to understand what's happening. This isn't autopilot. But the mechanical parts of development get handed off.

I've felt this kind of shift before. When I moved from FTP uploads to Git. When I started using VS Code after years of other editors. When hot reloading became standard. When QR codes went from "that weird square thing" to being everywhere. When Tailwind clicked and writing CSS the old way started feeling like unnecessary suffering. Each one felt like "oh, I'm not going back."

This feels like that.

## Getting started

If you want to try it: [claude.ai/code](https://claude.ai/code)

It needs API access or a Claude subscription. The learning curve is minimal. You just start talking to it in your project directory. The interesting part is figuring out how to describe what you want effectively, which is a skill that develops with use.
