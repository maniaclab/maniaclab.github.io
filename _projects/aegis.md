---
layout: project_page
title: AEGIS and OWL
group_title: Agentic Systems
tagline: Operational knowledge an agent can trust
lead: 'AEGIS puts AI agents into the day-to-day monitoring and operation of ATLAS distributed computing, and OWL is the knowledge layer underneath them: a curated, cited and versioned store of what is actually true about the systems, built so that agents and people draw on the same memory. Led by Ilija Vukotic.'
facts:
  - value: Claims, not chunks
    label: Every fact atomic, cited to a span of a stored document, with two timelines
  - value: Five MCP servers
    label: CRIC, Rucio, GGUS, Elasticsearch and OWL, on CERN login or an API key
  - value: Six specialist agents
    label: On two NVIDIA DGX Sparks, reporting daily into the collaboration's channels
  - value: ADC operations today
    label: The Analysis Facility and the Open Data Facility are the next extensions
figure:
  src: /assets/img/aegis-architecture.svg
  alt: "Diagram: people's assistants and three agent domains, Elwood for physics analysis, Shannon for facility operations and AEGIS for ATLAS distributed computing operations, all authenticate through one Keycloak realm federated to CERN SSO. They reach the facility through two MCP front doors, the AF MCP Platform gateway which brokers short-lived per-service credentials, and the AEGIS per-server endpoints which reach across ATLAS. Behind them are the facility's own services, the ATLAS distributed computing systems, and OWL's read tools. OWL's write path runs from sources, wikis and tickets and experts and agent memory, through a six-stage ingest pipeline into a Postgres claim store, where each claim carries span provenance, two timelines and a status. ATLAS distributed computing operations today, with the Analysis Facility and Open Data Facility planned."
  caption: One identity, two front doors, three agent domains and one shared knowledge layer. Maroon is the knowledge path; dashed is planned.
links:
  - title: maniaclab/AEGIS
    url: https://github.com/maniaclab/AEGIS
    note: MCP servers, OWL, deployment manifests and the agent state exporter. MIT licence.
  - title: OWL endpoint
    url: https://owl.af.atlas-ml.org/mcp
    note: Streamable HTTP. Sign in with CERN to contribute knowledge; API keys are read-only.
  - title: AF MCP Platform
    url: /projects/af-mcp-platform/
    note: The facility's credential-brokered gateway, the other front door in the diagram
  - title: Operations Analytics Platform
    url: /projects/operations-analytics-platform/
    note: The Elasticsearch platform the AEGIS agents reason over
people:
  - name: Ilija Vukotic
    url: /team/
    role: AEGIS and OWL lead; architecture, agents and the knowledge model
  - name: Giordon Stark
    url: /team/
    role: AF MCP platform and Elwood; the facility-side half of the same architecture
  - name: Fengping Hu
    url: /team/
    role: Analysis Facility operations agents and the Kubernetes platform they run on
  - name: Aidan Rosberg
    url: /team/
    role: RP1 and the Open Data Facility, where the knowledge layer extends next
  - name: Rob Gardner
    url: /team/
    role: Principal Investigator
status: The MCP servers are in production and the six agents report daily. OWL's ingest pipeline is live, with contradiction handling the next phase. Scope today is ATLAS distributed computing; the Analysis Facility and the Open Data Facility follow.
---

## The problem is not retrieval

Operational knowledge about ATLAS distributed computing lives in TWiki, GitHub, Indico, slide decks, GGUS tickets, mail, chat and people's heads. Three things are wrong with it at once. It is **stale**, because how Rucio, PanDA, FTS, CRIC and the sites behave changes faster than the pages describing it. It is **contradictory**, because several versions of the same fact coexist and nothing marks which is current — and sometimes both are right, in different situations. And the **why** behind a configuration change or a workaround is rarely written down at all.

Retrieval over that corpus does not fix any of it. Conventional RAG chunks the documents and asks the model to reconcile them at query time, every time, with no notion of which statement is current. For a person asking a question that is often good enough; they can weigh two answers. An operations agent needs the opposite: one answer, with high confidence, and an honest warning when confidence is not available.

So OWL moves the work from query time to **write time**. The hard part is maintaining a curated store. Once that exists, answering — with citations — is easy.

## Claims, not chunks

OWL does not store document chunks. It stores **claims**: atomic, self-contained statements, one per row, each about one entity.

> ATLAS DDM retries failed FTS transfers up to three times before marking the rule stuck.

Four properties do the work:

**Atomic and anchored.** One statement about one entity, with all that entity's aliases. Sites reuse their CRIC identity, which gives free disambiguation and a join path into live operational state; everything CRIC does not cover gets an entity in OWL's own namespace.

**Mandatory span provenance.** Every claim links to the exact text it came from, by byte range, in a stored original. No span, no claim — the extraction schema rejects it. Models do not copy quotes verbatim reliably, but they do copy line numbers, so the citation is the original text by construction.

**Two timelines.** When a claim was true is recorded separately from when OWL learned it. Nothing is ever deleted, only superseded. This is what makes obsolescence tractable: a newer claim about the same entity closes the old one's validity window and leaves a `supersedes` edge, so OWL can still answer "what did this site use before?"

**Status and decay.** Active, quarantined, disputed, superseded, retired — and a time-to-live that marks volatile facts stale so their owner re-attests or retires them. Every answer carries these flags, and an empty list means current, confirmed and fresh. A superseded claim is history, not an answer.

## Nobody has to say what is obsolete

Only what is true now. A new fact about the same entity closes the old one automatically. An expired claim is flagged stale in every answer until someone re-attests it. An owner can retire a claim with a reason. And when an agent notices a change while operating, that observation enters OWL quarantined and follows the same path.

Real conflicts are different from stale facts, and most apparent contradictions in a collaboration this size turn out to be **scope mismatches** rather than disputes: different site, different release, different data period, one person describing analysis and the other production. So resolution always offers "both true under different conditions", which adds qualifiers rather than retiring a true claim. Disputes route first to the two authors, then to a coordinator, and a resolution is itself a claim — so a question settled in 2026 cannot be quietly re-litigated in 2028.

## Writes are attributed, or they are worthless

Every writer resolves to a real person through CERN login. Trusted writers commit directly; everyone else lands in quarantine until confirmed. Shared API keys and service accounts are read-only by default. Agent memory is **always** quarantined, whoever submits it, because an agent that can write to shared knowledge can poison it by accident — a hallucinated detail helpfully saved must be traceable and reversible. Every write is rate-limited per identity and recorded in an append-only audit log.

Write tools never acknowledge into a black box either. Submitting knowledge answers with what happened — six claims extracted, two new, one conflicting with something a colleague asserted in June — so the submitter can correct it on the spot.

## Teaching it, and the Apprentice

The first approach was to give each system expert an agent and ask them to teach it. It did not really work: experts cannot tell what the agent already knows, teaching is real work on top of a day job, and important memories were compacted away as routine operations pushed fresh data in.

What replaced it is a knowledge store plus an **Apprentice** — an agent that wants to learn everything about ATLAS distributed systems and bootstraps itself by going out and studying them. It digests what it finds into claims, and fills the gaps by asking experts who have agreed to be asked. The questions are infrequent, deep, and answerable in five minutes.

The other agents are specialists: an orchestrator with architecture-level knowledge, and agents for networks, PanDA tasks, conditions delivery, Rucio data management and benchmark scores. They run on two NVIDIA DGX Sparks, file daily reports into the collaboration's channels, and their state is exported to git every day so what they learn can be reviewed rather than trusted blindly.

## The facility is the context

AEGIS is one of three agent domains the Lab runs, and they are better understood as three faces of one architecture than as three projects. **Elwood** carries a physicist's analysis. **Shannon** governs agents that help operate the Analysis Facility. **AEGIS** watches ATLAS distributed computing. What they share is more important than what separates them: one identity, a common way of reaching services, and — increasingly — one memory.

The architecture has **two front doors on purpose**. The [AF MCP Platform](/projects/af-mcp-platform/) is a credential-brokered gateway: one endpoint for a physicist's assistant, with the facility holding the credentials, minting short-lived per-service tokens and auditing every call. AEGIS instead publishes one endpoint per server, each with its own scope, because ADC operations has to be reachable from anywhere in ATLAS and not only from inside one facility. Both resolve callers through the same Keycloak realm, federated to CERN SSO, so the two doors differ in reach and credential handling, not in who you are.

That is the practical content of "the facility is the context". A generic model knows a great deal about Rucio in the abstract and nothing about this site's quirks, last month's workaround, or which of two contradictory wiki pages is current. Closing that gap is not a prompt-engineering problem. It needs the facility to carry its own context — brokered credentials for acting, and curated, cited knowledge for knowing — and to offer both through the same interface an agent already speaks.

## What comes next

OWL's scope today is ATLAS distributed computing operations. The claim model is not specific to it: claims, provenance and two timelines fit any operations knowledge, and the same store is intended to carry **Analysis Facility** knowledge — what users need to know about the facility they are working on — and **Open Data Facility** knowledge, where an external audience with no collaboration membership needs accurate, citable answers most of all.

The open questions are organisational rather than technical, and they get harder as the scope widens: who counts as a trusted writer in a collaboration of thousands, and whether a shared knowledge layer should be one store or federated ones.
