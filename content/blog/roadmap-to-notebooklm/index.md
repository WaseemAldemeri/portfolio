---
title: "Automating Roadmap.sh into NotebookLM"
date: 2026-03-25T14:00:00+03:00
description: "How I bridged the gap between the world's best developer roadmaps and AI-powered study tools."
summary: "Separately, Roadmap.sh and NotebookLM are gems. Together, they are insane. Here is how I used Python to automate my entire learning workflow."
tags: ["python", "automation", "notebooklm", "roadmap-sh"]
categories: ["projects"]
showTableOfContents: true
---

## Introduction

If you're a developer, you probably know [roadmap.sh](https://roadmap.sh). It’s the gold standard for figuring out what to learn, packed with community-curated articles and videos.

Then there’s [NotebookLM](https://notebooklm.google.com). For me, this is the "holy grail" of studying. You feed it a few links, and it generates these incredibly fun, informative "Deep Dive" podcasts. I started listening to them on my daily commute, and honestly, I've never absorbed complex tech topics faster.

**The Problem:** The Backend roadmap has over **130 nodes**. Each node has about 7 resources.
Doing the math? That’s nearly 1,000 links to manually copy-paste.

I tried doing it by hand for about ten minutes before realizing I’d be there all week. So, I did what any developer would do: **I automated it**.

{{< github repo="WaseemAldemeri/roadmap-to-notebooklm" >}}

## Seeing it in Action

{{< video src="roadmap-to-notebooklm.mp4" controls=true autoplay=false loop=false ratio="16/9" >}}

[Example of a generated notebook about gRPC](https://notebooklm.google.com/notebook/09feeef9-b604-46b5-84dc-e6d49e64ee6f)

## How I Built the Bridge

I wanted a simple, three-stage pipeline that didn't over-engineer the problem. I used a community-built wrapper called [notebooklm-py](https://github.com/teng-lin/notebooklm-py) to handle the heavy lifting with Google’s internal API.

{{< mermaid >}}
flowchart TD
A[(roadmap.sh\nGitHub Repo)]
A -->|Scrape Links| B[/Stage 1\nget_resources.py/]
B -->|JSON Map| C[/Stage 2\ncreate_notebooks.py/]
C -->|Auto-Upload| D{{NotebookLM}}
D -->|Fuzzy Search CLI| E[/Stage 3\ngenerate_study_material.py/]
E --> F1([Flashcards])
E --> F2([Podcasts])
E --> F3([Quizzes])

    classDef source fill:#1e3a5f,stroke:#4a9eff,color:#e0f0ff
    classDef stage fill:#1a2e1a,stroke:#4caf50,color:#b8f0b8
    classDef platform fill:#3b1f4e,stroke:#a855f7,color:#e9d5ff
    classDef output fill:#2e1f1a,stroke:#f97316,color:#fed7aa

    class A source
    class B,C,E stage
    class D platform
    class F1,F2,F3 output

{{< /mermaid >}}

### 1. The Scraper (`get_resources.py`)

First, I needed the data. I targeted the open-source repo behind roadmap.sh. The script recursively digs through their JSON structures and markdown files to find every curated YouTube video and article link.

### 2. The Heavy Lifting (`create_notebooks.py`)

This is where the magic happens. The script logs into NotebookLM (reusing a local session) and:

- Creates a dedicated notebook for every single topic.
- Generates a "context" markdown file so the AI knows _why_ it's looking at these links.
- Uploads the resources automatically.

_Note: Google doesn't love it when you create 130 notebooks in 2 seconds, so I added some deliberate `asyncio.sleep` pauses to keep things civil._

### 3. The Interactive CLI (`generate_study_material.py`)

Since you can't (and shouldn't) generate 130 podcasts at once, I built a snappy CLI. It uses `InquirerPy` for **fuzzy searching**.

When I want to study "Redis" or "gRPC," I just type a few letters, select the topic, and hit Enter. The script triggers the generation on Google’s servers, and by the time I’ve grabbed a coffee, my study materials are ready.

## The Takeaway

I’ve learned more in the last few weeks using this setup than I have in months of "doom-scrolling" documentation.

If you want to set this up for yourself (it works for Frontend, DevOps, or any other roadmap too), check out the [GitHub repo](https://github.com/WaseemAldemeri/roadmap-to-notebooklm)!
