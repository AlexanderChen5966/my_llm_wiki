---
title: "Agentic Engineering Patterns - Simon Willison's Weblog"
source: "https://simonwillison.net/guides/agentic-engineering-patterns/"
author:
  - "[[Simon Willison]]"
published:
created: 2026-05-19
description:
tags:
  - "clippings"
status: processed
---
## Agentic Engineering Patterns

Patterns for getting the best results out of coding agents like Claude Code and OpenAI Codex. See [my introduction](https://simonwillison.net/2026/Feb/23/agentic-engineering-patterns/) for more on this project.

1. **Principles**
	1. [What is agentic engineering?](https://simonwillison.net/guides/agentic-engineering-patterns/what-is-agentic-engineering/)
		- [Agentic engineering](https://simonwillison.net/guides/agentic-engineering-patterns/what-is-agentic-engineering/#agentic-engineering)
				- [Isn't this just vibe coding?](https://simonwillison.net/guides/agentic-engineering-patterns/what-is-agentic-engineering/#isnt-this-just-vibe-coding)
				- [About this guide](https://simonwillison.net/guides/agentic-engineering-patterns/what-is-agentic-engineering/#about-this-guide)
		2. [Writing code is cheap now](https://simonwillison.net/guides/agentic-engineering-patterns/code-is-cheap/)
		- [Good code still has a cost](https://simonwillison.net/guides/agentic-engineering-patterns/code-is-cheap/#good-code)
				- [We need to build new habits](https://simonwillison.net/guides/agentic-engineering-patterns/code-is-cheap/#we-need-to-build-new-habits)
		3. [Hoard things you know how to do](https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/)
		- [Recombining things from your hoard](https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/#recombining-things-from-your-hoard)
				- [Coding agents make this even more powerful](https://simonwillison.net/guides/agentic-engineering-patterns/hoard-things-you-know-how-to-do/#coding-agents-make-this-even-more-powerful)
		4. [AI should help us produce better code](https://simonwillison.net/guides/agentic-engineering-patterns/better-code/)
		- [Avoiding taking on technical debt](https://simonwillison.net/guides/agentic-engineering-patterns/better-code/#avoiding-taking-on-technical-debt)
				- [Coding agents can handle these for us](https://simonwillison.net/guides/agentic-engineering-patterns/better-code/#coding-agents-can-handle-these-for-us)
				- [AI tools let us consider more options](https://simonwillison.net/guides/agentic-engineering-patterns/better-code/#ai-tools-let-us-consider-more-options)
				- [Embrace the compound engineering loop](https://simonwillison.net/guides/agentic-engineering-patterns/better-code/#embrace-the-compound-engineering-loop)
		5. [Anti-patterns: things to avoid](https://simonwillison.net/guides/agentic-engineering-patterns/anti-patterns/)
		- [Inflicting unreviewed code on collaborators](https://simonwillison.net/guides/agentic-engineering-patterns/anti-patterns/#inflicting-unreviewed-code-on-collaborators)
2. **Working with coding agents**
	1. [How coding agents work](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/)
		- [Large Language Models](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#large-language-models)
				- [Chat templated prompts](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#chat-templated-prompts)
				- [Token caching](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#token-caching)
				- [Calling tools](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#calling-tools)
				- [The system prompt](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#the-system-prompt)
				- [Reasoning](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#reasoning)
				- [LLM + system prompt + tools in a loop](https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/#llm-system-prompt-tools-in-a-loop)
		2. [Using Git with coding agents](https://simonwillison.net/guides/agentic-engineering-patterns/using-git-with-coding-agents/)
		- [Git essentials](https://simonwillison.net/guides/agentic-engineering-patterns/using-git-with-coding-agents/#git-essentials)
				- [Core concepts and prompts](https://simonwillison.net/guides/agentic-engineering-patterns/using-git-with-coding-agents/#core-concepts-and-prompts)
				- [Rewriting history](https://simonwillison.net/guides/agentic-engineering-patterns/using-git-with-coding-agents/#rewriting-history)
		3. [Subagents](https://simonwillison.net/guides/agentic-engineering-patterns/subagents/)
		- [Claude Code’s Explore subagent](https://simonwillison.net/guides/agentic-engineering-patterns/subagents/#claude-codes-explore-subagent)
				- [Parallel subagents](https://simonwillison.net/guides/agentic-engineering-patterns/subagents/#parallel-subagents)
				- [Specialist subagents](https://simonwillison.net/guides/agentic-engineering-patterns/subagents/#specialist-subagents)
				- [Official documentation](https://simonwillison.net/guides/agentic-engineering-patterns/subagents/#official-documentation)
3. **Testing and QA**
	1. [Red/green TDD](https://simonwillison.net/guides/agentic-engineering-patterns/red-green-tdd/)
		2. [First run the tests](https://simonwillison.net/guides/agentic-engineering-patterns/first-run-the-tests/)
		3. [Agentic manual testing](https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/)
		- [Mechanisms for agentic manual testing](https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/#mechanisms-for-agentic-manual-testing)
				- [Using browser automation for web UIs](https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/#using-browser-automation-for-web-uis)
				- [Have them take notes with Showboat](https://simonwillison.net/guides/agentic-engineering-patterns/agentic-manual-testing/#have-them-take-notes-with-showboat)
4. **Understanding code**
	1. [Linear walkthroughs](https://simonwillison.net/guides/agentic-engineering-patterns/linear-walkthroughs/)
		- [An example using Showboat and Present](https://simonwillison.net/guides/agentic-engineering-patterns/linear-walkthroughs/#an-example-using-showboat-and-present)
		2. [Interactive explanations](https://simonwillison.net/guides/agentic-engineering-patterns/interactive-explanations/)
		- [Understanding word clouds](https://simonwillison.net/guides/agentic-engineering-patterns/interactive-explanations/#understanding-word-clouds)
5. **Annotated prompts**
	1. [GIF optimization tool using WebAssembly and Gifsicle](https://simonwillison.net/guides/agentic-engineering-patterns/gif-optimization/)
		- [The follow-up prompts](https://simonwillison.net/guides/agentic-engineering-patterns/gif-optimization/#the-follow-up-prompts)
		2. [Adding a new content type to my blog-to-newsletter tool](https://simonwillison.net/guides/agentic-engineering-patterns/adding-a-new-content-type/)
6. **Appendix**
	1. [Prompts I use](https://simonwillison.net/guides/agentic-engineering-patterns/prompts/)