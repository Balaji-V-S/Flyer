<a href="https://balajivs.me"><img src="https://github.com/Balaji-V-S/Balaji-V-S/blob/main/Github%20Readme%20watermark.png" align="right" width="140" /></a>

# Flyer Core — Self-Hostable Micro-Knowledge Engine

**Flyer Core** is a blazing-fast, self-hostable backend that ingests content from any source, distills it into bite-sized knowledge, and serves it through clean APIs.  
Built with **Spring Boot** and compiled using **GraalVM Native Image**, Flyer Core runs on any container or VPS or your own homelab 

It’s not an app.  
It’s *infrastructure for knowledge*.

---

## What Flyer Core Does

- Ingests content from:
  - RSS feeds
  - Web URLs
  - Files / APIs
- Runs a pluggable processing pipeline:
  - Normalize → Summarize → Tag → De-duplicate
- Optionally uses LLMs to:
  - Create one-line knowledge cards
  - Generate summaries
  - Auto-tag topics
- Stores and serves:
  - Knowledge cards
  - Feeds
  - Topics
  - History & metadata
- Exposes a clean REST API for:
  - Web apps
  - Mobile apps
  - Widgets
  - Bots
  - Any client

You bring the frontend.  
Flyer Core powers it.

---

## Key Features

- 🚀 **GraalVM Native** – Instant startup, ultra-low memory
- 🏗️ **Self-Hostable** – Run it on your own server, NAS, or edge device
- 🔌 **Pluggable Pipeline** – Swap or extend ingestion & processing stages
- 🧠 **AI-Ready** – Optional LLM integration for summarization & tagging
- 🧩 **API-First** – Clean REST APIs for any client
- 🐳 **Single-Binary Deploy** – Tiny Docker image, production-ready

---

## Tech Stack

- **Backend:** Spring Boot (Java 21)
- **Runtime:** GraalVM Native Image
- **Storage:** MongoDB / PostgreSQL / Embedded DB (configurable)
- **AI Layer:** OpenAI / Ollama / Local LLMs (optional)
- **Deployment:** Native binary, Docker, or bare-metal

---

Everything is modular, replaceable, and Graal-friendly.

---

## Use Cases

- Personal knowledge server  
- Daily knowledge feed for apps  
- Internal learning system for teams  
- Newsletter backend  
- Edge-deployed info engine  

---

## Vision

Flyer Core aims to be the **Frappe of knowledge infrastructure** —  
a lightweight, open, self-hostable engine that turns the chaos of the internet into calm, consumable insight.

Not another app.  
A *foundation* for how knowledge flows.


