# Flyer Core --- Architecture

Flyer Core is designed as a **self-hostable knowledge infrastructure
system**.\
It follows an *ingest → process → store → serve* model, with CLI-first
control and optional lightweight UI.

The system behaves like an infra tool (Git, Redis, Docker), not a
consumer app.

------------------------------------------------------------------------

## High-Level Flow

    [ Sources ]
         │
         ▼
    [ Ingestors ]
         │
         ▼
    [ Processing Pipeline ]
         │
         ▼
    [ Knowledge Store ]
         │
         ▼
    [ REST API ]
         │
         ▼
    [ Any Client ]
    (Web, App, Bot, Widget, etc.)

------------------------------------------------------------------------

## Core Components

### 1. Ingest Layer

Responsible for fetching raw content.

``` java
public interface Ingestor {
    List<RawItem> fetch();
}
```

Implementations: - `RssIngestor` - `WebIngestor` - `FileIngestor` -
`ApiIngestor`

Each produces:

``` java
class RawItem {
    String source;
    String title;
    String content;
    String url;
    Instant fetchedAt;
}
```

------------------------------------------------------------------------

### 2. Processing Pipeline

A chain of modular stages.

``` java
public interface Stage<I, O> {
    O process(I input);
}
```

Default stages:

1.  `NormalizeStage` -- clean & sanitize content\
2.  `SummarizeStage` -- create one-line insight (LLM or heuristic)\
3.  `TagStage` -- extract topics\
4.  `DedupStage` -- remove duplicates

Flow:

    RawItem → CardDraft → KnowledgeCard

Each stage is replaceable and configurable via `flyer.yml`.

------------------------------------------------------------------------

### 3. Storage Layer

Stores final knowledge artifacts.

``` java
class KnowledgeCard {
    UUID id;
    String text;
    String source;
    List<String> tags;
    Instant createdAt;
}
```

Repository interface:

``` java
interface CardRepository {
    void save(KnowledgeCard card);
    List<KnowledgeCard> latest(int n);
}
```

Adapters: - Embedded DB (default) - MongoDB - PostgreSQL

------------------------------------------------------------------------

### 4. Scheduler

Runs the pipeline periodically.

``` java
@Scheduled(cron = "0 0 * * * ?")
void runPipeline();
```

Flow:

    For each source:
       fetch → pipeline → store

Also triggerable via CLI:

``` bash
flyer ingest now
```

------------------------------------------------------------------------

### 5. API Layer

Exposes clean REST endpoints.

    GET  /api/cards/latest
    GET  /api/cards/{id}
    GET  /api/topics
    POST /api/ingest/run
    GET  /health

Any client can consume these APIs.

------------------------------------------------------------------------

### 6. CLI (Primary Interface)

Single binary using `picocli`.

    flyer init
    flyer run
    flyer source add <url>
    flyer source list
    flyer ingest now
    flyer cards tail
    flyer status

The CLI: - Creates and edits config - Controls runtime - Talks to API
when server is running

------------------------------------------------------------------------

### 7. Optional Admin UI

-   Lightweight HTML
-   HTMX / Alpine.js
-   Served from `/ui/*`
-   Purpose:
    -   Add sources
    -   View cards
    -   Check system health

No SPA. No heavy frameworks.

------------------------------------------------------------------------

## Runtime Model

1.  Load configuration\
2.  Register ingestors\
3.  Build pipeline\
4.  Initialize storage\
5.  Start REST API\
6.  Start scheduler

Binary modes:

``` bash
flyer init   # create config
flyer run    # start server
```

------------------------------------------------------------------------

## Philosophy

-   Self-hostable by default\
-   Single native binary\
-   Config-driven\
-   CLI-first\
-   Frontend is optional\
-   Owner controls data

Flyer Core is **infrastructure for knowledge**, not an app.
