---
title: "The Open Knowledge Format (OKF) from Google is a new layer for agents"
source: "https://www.mariehaynes.com/okf/"
author:
  - "[[Marie Haynes]]"
published: 2026-06-16
created: 2026-06-22
description: "Google just published documentation on what they're calling the Open Knowledge Format (OKF). It represents a standardized way for agents to access knowledge via very simple directories of Markdown files. While storing files in Markdown is nothing new, Google is now creating a specific standard that agents can understand without the need for specific software […]"
tags:
  - "clippings"
status: processed
---
Google just published documentation on what they're calling the [Open Knowledge Format (OKF)](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing/). It represents a standardized way for agents to access knowledge via very simple directories of Markdown files. While storing files in Markdown is nothing new, Google is now creating a specific standard that agents can understand without the need for specific software or other platforms.

While the examples Google gives are mostly for using agents to analyze your data (i.e. from BigQuery for example), I think OKF represents so much more. It's quite possible that every business will organize its knowledge - both public and for use inside of the company - into an Open Knowledge Format, making it incredibly easy for agents to do things with it.

Follow along as I walk through Google's documentation in this video:

## The shift from SEO to agentic accessibility

This new format may change how we approach SEO. We will shift from working to be found by search engines to making business knowledge accessible so agents can perform tasks with it. I suspect we will see high demand for OKF specialists who understand how to turn a company's mess of data and processes into a clean knowledge graph. We might need a new acronym what we do because this really isn't SEO, GEO or even Agentic Search Optimization!

## Understanding OKF knowledge bundles

An [OKF bundle is just a directory of Markdown files](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md). Each file represents a single concept or unit of knowledge. Rather than turning every *web page* into a Markdown file, you extract specific *concepts*. A single webpage may end up producing quite a few concept markdown files.

These bundles start with meta data known as YAML frontmatter, which tells the agent the type of content, the title, and relevant tags. This allows agents to quickly parse what a file is about without reading the entire text first.

![yaml-frontmatter](https://www.mariehaynes.com/wp-content/uploads/2026/06/yaml-frontmatter.webp)

*(These examples are from Google's [spec.md file for OKF](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md).)*

Following the YAML frontmatter is the body. It can contain *anything.* It is written in markdown and can use headings,, lists, tables, code blocks and more.

![okf markdown](https://www.mariehaynes.com/wp-content/uploads/2026/06/okf-markdown.webp)

So basically the YAML frontmatter is like the index card that tells agents what is in this bundle and the body is where your wisdom, data and processes are stored. The fascinating thing is that there is no standard format for the body. You can put whatever you want in here. Because the format for finding information is standardized, agents will know how to find exactly what they need and possibly even find information on what to do with that information, assuming your company processes are also documented in your knowledge file.

This system means that agents don't need to stuff the entirety of your knowledge into their context window and do RAG across it. Rather, they can look at the format and figure out exactly which parts of the knowledge will help them.

## The LLM Wiki pattern

This format formalizes the [LLM-Wiki pattern described by Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). The idea is that instead of just retrieving raw documents, an agent **builds and maintains a persistent wiki of your knowledge**. When you add new information, your agent does not just index it. It reads it, extracts key insights, and integrates them into an existing knowledge base. It can even note where new data contradicts old claims, creating a living synthesis of your expertise. It can links related concepts together, creating a knowledge base for your business. Karpathy also describes a Lint function that periodically reviews your system to find contradictions or things that need updating.

## Prediction: SEOs who can make good OKF bundles will be in demand

There are already tools in existence that can take your webpages and turn them into an OKF bundle. [Suganthan Mohanadasan has one here](https://suganthan.com/blog/open-knowledge-format/). It's worth playing with!

However, I think it will take significant skill to create a bespoke OKF for a company. You will need to thoroughly understand a business, the concepts they have intelligence on, their processes that can be documented and everything else that should be put into the open knowledge format bundle. You will be building a brain for that business and not just markdown versions of pages. The brain may be used internally, or parts of it might be sold to others and incorporated into the intelligence of other businesses who can utilize their intelligence. And there will be need for maintaining this brain as it evolves and as the OKF standard evolves.

This is why I'd encourage you to play around with creating OKF bundles - so you can develop expertise. Keep in mind the standard is new and will likely change.

## New revenue streams for expert knowledge

One of the most exciting aspects of OKF is the ability to sell knowledge bundles. *I should mention that this is not specifically mentioned in Google's documentation. This is what I am predicting will happen with OKF.* Imagine a lawyer, accountant, or SEO professional selling an OKF bundle of their proprietary processes. You could purchase these bundles and integrate them directly into your own system. This moves us toward a world where those with deep expertise are rewarded for sharing structured knowledge that agents can immediately put to work.

## My early attempts at creating an OKF out of my data

I gave the OKF documentation to Antigravity and played around with creating an OKF version of the information I have written recently (mostly internally, not public) in my [traffic drop assessments](https://www.mariehaynes.com/traffic-drop-assessment/). The system I made extracted the important concepts from several documents and stored each as a markdown file. I could then visualize them in a graph, seeing which concepts relate to each other.

![okf concept](https://www.mariehaynes.com/wp-content/uploads/2026/06/needs-met-review-screen.webp)

Each of these nodes in the graph below is a single concept that was pulled out of my writing. Some concepts relate to each other.

![okf knowledge graph](https://www.mariehaynes.com/wp-content/uploads/2026/06/okf-knowledge-graph.webp)

I could then use an LLM that queried this OKF. In this case I used Gemini-3-flash-preview. (At this point I think this is the best model factoring in speed, accuracy and cost.)

![](https://www.mariehaynes.com/wp-content/uploads/2026/06/core-update-brain-assistant.webp)

Please note this is a very early test version that has only ingested 3 training documents that I wrote. I will be greatly improving this system over time. I also want to experiment with using Google's [Agent Development Kit](https://adk.dev/) to create agents that can do things with my knowledge. I would strongly encourage you to try building an okf yourself!

## Getting started with OKF

OKF helps us move away from having to format everything for machines. Instead, we can focus on learning and sharing insights while the agents handle the categorization. What I did was put these links below into NotebookLM and then I spent time in the Gemini app using the Notebook as a source. Eventually when I understood the concepts, I moved to Antigravity and used the NotebookLM MCP to give the necessary information to Antigravity. You could also just forgo the Notebook and give Antigravity the github links directly. In hindsight this is what I will do next time. Reading a summary of the github repository was helpful for me to brainstorm with Gemini, but accessing the repository itself seems to be better for Antigravity.

**Google's documentation on OKF:**

[https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing/](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing/)

**Google's information on Github on the spec.md file for OKF:**

[https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)

**Andrej Karpathy on LLM-Wiki:**

[https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

You can try this prompt in Gemini. It is especially good if you have memory turned on.

*Here is a Notebook (or here are links) with information from Google on the Open Knowledge Format. Help me understand how I could use this for my life and business. Give me 20 ideas of things I could create using this format.*

Or, [join us in the Search Bar](https://community.mariehaynes.com/spaces/11879397/feed) for up to the minute news on Search and AI. Or better yet, [join my paid community](https://www.mariehaynes.com/join/) where we have regular calls to brainstorm on using AI and I share tips and ideas as well.

#### Recent conversations in the Search Bar Pro Paid community:

- [💬 Ask AI Mode About Your EEAT (and How to Improve It)](https://community.mariehaynes.com/posts/ask-ai-mode-about-your-eeat-and-how-to-improve-it)
- [💬 Understanding UCP](https://community.mariehaynes.com/posts/102188334)
- [💬 A Prompt for Overly Complicated AI Chats](https://community.mariehaynes.com/posts/102186765)
- [💬 Using Gemini to Simplify Core Web Vital Improvements](https://community.mariehaynes.com/posts/use-gemini-in-the-chrome-sidebar-to-simplify-core-web-vital-improvements)
- [💬 Is SaaS Dead? An Interesting Read](https://community.mariehaynes.com/posts/102102572)
- [💬 What If AI Agents Took Over the World?](https://community.mariehaynes.com/posts/102094880)
- [💬 Using Antigravity to Improve AIO Presence](https://community.mariehaynes.com/posts/use-antigravity-to-improve-aio-presence-and-stay-within-googles-guidelines)
- [📓 Marie's full 57 page guide + video for doing amazing things with Antigravity](https://community.mariehaynes.com/posts/maries-guide-to-using-antigravity-march-2026)