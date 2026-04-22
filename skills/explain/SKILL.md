---
name: explain
description: Explain any concept, system, codebase, technology, or process using a structured three-section format -- High-Level overview, Break-Down, and Glossary. Use this skill whenever the user asks you to explain, describe, walk through, or help them understand something -- whether it's a piece of code, an architecture, a protocol, a framework, a design pattern, an algorithm, a business process, or any technical or non-technical topic. Also use this skill when the user says things like "what is...", "how does...work", "can you break down...", "help me understand...", "ELI5", or "walk me through...". Even if the user doesn't explicitly say "explain", if the intent is clearly to understand something rather than to build or fix something, use this skill.
---

# Explain Skill

When explaining anything, always structure the response into exactly three sections in this order:

1. **High-Level**
2. **Break-Down**
3. **Glossary**

Each section serves a distinct purpose. Together they give the reader a complete picture -- from the 30,000-foot view down to every piece of jargon they might not know.

## 1. High-Level

The goal here is to give the reader a quick mental model of what the thing is and why it matters, before diving into any details. Think of it as the "elevator pitch" version of the explanation.

**Format rules:**

- Use bullet points -- no walls of text
- Keep each bullet to 1-2 sentences max
- Include a flow diagram or visual whenever it helps show how pieces relate or how a process flows
- Focus on the _what_ and the _why_, not the _how_ (that's for the Break-Down)
- Aim for 3-7 bullets -- enough to orient, not enough to overwhelm

**Flow diagrams:** Use ASCII/text-based diagrams liberally. They are extremely effective at showing relationships, data flow, architecture layers, or process steps at a glance.

**Example structure:**

```
## High-Level

- REST is a style of designing web APIs where each URL represents a resource
- Clients use standard HTTP methods (GET, POST, PUT, DELETE) to interact with resources
- The server is stateless -- every request carries all the info needed to process it

+--------+     HTTP Request      +---------+     Query     +------+
| Client | ------------------->  |  Server | ------------> |  DB  |
|        | <-------------------  |         | <------------ |      |
+--------+     HTTP Response     +---------+    Results    +------+
```

## 2. Break-Down

This is where you decompose the topic into its constituent parts and explain how each part works. Go deeper than the High-Level section, but stay concise -- bullet points and diagrams, not essays.

**Format rules:**

- Use bullet points throughout
- Group related bullets under sub-headings if the topic has distinct components or phases
- Include diagrams wherever they clarify relationships, sequences, or data transformations
- Show cause-and-effect: "X happens, which triggers Y, which produces Z"
- If there's a process or lifecycle, show it as a numbered sequence or flow diagram
- If there are components, show how they connect

**When to use diagrams:**

- Processes with steps --> flow diagram
- Components with relationships --> architecture/block diagram
- Data transformations --> pipeline diagram
- Decision logic --> decision tree
- Hierarchies --> tree diagram
- Timelines/sequences --> sequence diagram

**Example structure:**

```
## Break-Down

### Request Lifecycle

1. Client constructs the HTTP request
2. DNS resolves the domain to an IP
3. TCP connection established (TLS handshake if HTTPS)
4. Server receives and routes the request
5. Handler processes the request and returns a response

Client          DNS           Server          DB
  |   resolve     |              |              |
  |-------------->|              |              |
  |   IP addr     |              |              |
  |<--------------|              |              |
  |        GET /users            |              |
  |----------------------------->|              |
  |              |               |  SELECT *    |
  |              |               |------------->|
  |              |               |   rows       |
  |              |               |<-------------|
  |        200 OK + JSON         |              |
  |<-----------------------------|              |

### HTTP Methods

- **GET** - retrieve a resource; safe and idempotent
- **POST** - create a new resource; not idempotent
- **PUT** - replace a resource entirely; idempotent
- **PATCH** - partially update a resource
- **DELETE** - remove a resource; idempotent
```

## 3. Glossary

The audience is a junior software engineer. They know general programming concepts (variables, functions, APIs, HTTP, databases, CLI, OS, CPU, RAM, etc.) but may not know domain-specific terminology for the topic being explained. The glossary should only define terms that are specific to the topic or that a junior engineer would genuinely not know. Skip general computing terms they already understand.

**What to include:**

- Domain-specific jargon and concepts (e.g., "cgroups", "namespace", "idempotent", "event sourcing")
- Non-obvious acronyms specific to the domain (e.g., "CQRS", "OOM", "UTS")
- Named tools/products the reader may not have encountered (e.g., "containerd", "runc")
- Terms with a specific meaning in this context that differs from everyday usage (e.g., "image" in Docker, "HEAD" in Git)

**What to skip:**

- General programming/CS basics (API, HTTP, DNS, TCP, CLI, OS, CPU, RAM, JSON, SQL, etc.)
- Common dev tools everyone knows (Git, Docker, npm -- unless they are the subject being explained)
- Everyday English words used in their normal sense

**Format rules:**

- Use bullet points: **`term`** -- definition
- Sort alphabetically using case-insensitive ordering. Double-check the ordering after writing
- Keep definitions to 1-2 sentences -- concise but complete
- If a term has a common abbreviation, include both forms
- For programming keywords/reserved words, use code formatting
- Aim for 10-20 terms -- enough to cover the domain-specific language without padding with basics

**Example structure:**

```
## Glossary

- **cgroups (control groups)** -- a Linux kernel feature that limits and accounts for the resource usage of a collection of processes
- **Copy-on-write** -- a strategy where shared data is only duplicated when one process tries to modify it, saving memory and disk space
- **Idempotent** -- an operation that produces the same result no matter how many times you repeat it
- **Namespace** -- a Linux kernel feature that partitions system resources so each partition appears to be an independent system
- **Stateless** -- a design where the server stores no information about previous requests; each request is self-contained
- **Union filesystem** -- a filesystem that layers multiple directories on top of each other, presenting them as a single merged view
```

## General Guidelines

- **Conciseness over completeness.** Every bullet should earn its place. If a point doesn't help the reader understand the topic, cut it.
- **Diagrams are not optional.** If the topic involves any kind of flow, relationship, process, hierarchy, or sequence, include a visual. ASCII diagrams are preferred since they render everywhere.
- **Adapt depth to complexity.** A simple concept like "what is a variable" needs a shorter Break-Down than "how does Kubernetes networking work". Scale the sections proportionally.
- **Glossary should cover domain-specific terms only.** After writing all three sections, re-read the High-Level and Break-Down and check that every domain-specific term, non-obvious acronym, or context-specific keyword has a Glossary entry. Do not pad the glossary with general computing terms a junior software engineer already knows (API, HTTP, CLI, etc.). The glossary should feel targeted, not exhaustive.
- **When explaining code:** include short, focused code snippets in the Break-Down section alongside the bullets. Don't dump entire files -- show the relevant 3-10 lines.
- **When explaining systems/architectures:** the High-Level diagram should show the major components and their connections. The Break-Down diagrams should zoom into individual components or interactions.
