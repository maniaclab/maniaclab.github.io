---
layout: facility_page
title: Open Data Facility
group_title: Open Data
order: 4
icon: database
tagline: A United States home for ATLAS Open Data
summary: A mirror of ATLAS Open Data in the US, with notebooks and reproducible workflows attached, so that students, educators and researchers can work with real LHC data without an experiment account.
lead: Most of the computing the Lab operates is reachable only by members of an experiment. The Open Data Facility is the exception. It holds a United States copy of ATLAS Open Data and puts working environments directly beside it, so that the barrier to doing something real with LHC data is a browser rather than a collaboration membership.
facts:
  - value: 1 PB
    label: Dedicated dCache storage, separate from the production grid pools
  - value: Runs on RP1
    label: The Lab's multi-site research platform at Chicago and Indiana
  - value: Notebooks and workflows
    label: BinderHub and JupyterHub sessions, plus reproducible REANA workflows
  - value: Built with CLARIPHY
    label: Developed with the CLARIPHY Open Data and AI working group
links:
  - title: RP1
    url: https://rp1.hl-lhc.io
    note: The research platform the facility runs on
  - title: CLARIPHY
    url: /collaborations/clariphy/
    note: The community laboratory the facility is being developed with
  - title: ATLAS at MANIAC Lab
    url: /collaborations/atlas/
    note: The experiment whose open data is mirrored here
  - title: Facilities and Platforms
    url: /projects/#facilities
    note: The platforms and services we develop on this infrastructure
people:
  - name: Aidan Rosberg
    url: /team/
    role: Infrastructure owner of the RP1 research platform
  - name: Fengping Hu
    url: /team/
    role: Kubernetes platform engineering and the hosted notebook services
  - name: Giordon Stark
    url: /team/
    role: Physicist-facing platform and analysis tooling
status: In development with the CLARIPHY Open Data and AI working group, building on the Lab's long involvement in data preservation and reuse through DASPOS and REANA.
---

## Why it exists

ATLAS releases a portion of its data publicly, for education, outreach and research that does not require collaboration membership. Getting value from it is a different matter: open data is only useful if there is somewhere to put it and something to run against it. Downloading hundreds of gigabytes to a laptop is not an analysis environment.

The Open Data Facility closes that gap. It mirrors ATLAS Open Data in the United States, which removes a transatlantic hop for anyone working with it here, and it attaches the same kinds of working environment the Lab's other facilities provide, so the data and the compute sit next to each other.

## What is here

Storage is a dedicated petabyte of dCache, deliberately kept separate from the production grid storage pools so that public use and experiment production cannot interfere with each other. Alongside it sits one of the Lab's large archival storage servers and a small set of worker nodes.

The working environments are the same ones the Lab runs elsewhere, pointed at open data instead of experiment data. BinderHub turns a Git repository into a running notebook session, which makes a tutorial or a paper's analysis code something a reader can execute rather than only read. JupyterHub provides longer-lived interactive sessions. REANA runs workflows that are meant to be reproducible, capturing the software environment along with the steps.

All of this runs on [RP1](https://rp1.hl-lhc.io), the Lab's multi-site research platform spanning the University of Chicago and Indiana University, which means the facility inherits that platform's identity, scheduling and declarative management rather than needing its own.

## Who it is for

Three audiences, with different needs. Educators and students, who need something that works in a classroom session without an account request. Outreach, where the point is that the data is genuinely the experiment's. And researchers working on AI-era methods, for whom open data is often the only corpus that can be shared, benchmarked against and published openly.

That last group is why the facility is being built with the [CLARIPHY](/collaborations/clariphy/) Open Data and AI working group. Open physics data turns out to matter a great deal for machine learning research, where reproducibility and shared benchmarks depend on everyone being able to reach the same data.

## Lineage

The Lab has been working on this problem for a long time. DASPOS, an NSF project with Notre Dame, Michigan and others, was an early attempt to work out what it takes to preserve not just data but the software and context needed to understand and trust it. REANA, which the Lab helped deploy and now runs, came out of the same line of thinking. The Open Data Facility is where those ideas meet actual public data and actual users.
