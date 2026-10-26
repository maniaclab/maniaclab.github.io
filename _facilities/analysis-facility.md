---
layout: facility_page
title: UChicago Analysis Facility
group_title: Analysis Computing
order: 2
icon: laptop
tagline: Interactive analysis at scale for U.S. ATLAS
summary: In production since 2021, the Analysis Facility gives ATLAS physicists login nodes, notebooks, batch computing, GPUs and petabytes of storage, built on Kubernetes and designed around how people actually work.
lead: The Analysis Facility was built for a different problem than a grid site. Physicists do not submit an analysis and come back in a week; they iterate, interactively, on data they want close at hand. The facility is designed around that, with user interactivity, newer analysis workflows and emerging technologies as the organizing principles, and Kubernetes as the substrate that makes change cheap. It is open to any member of U.S. ATLAS and their collaborators, using their own institution's credentials.
facts:
  - value: 8,600 cores
    label: Local batch capacity, with 13,000 more reachable by flocking to MWT2
  - value: 5.2 PB
    label: Ceph-backed user storage, about 1.7 PB usable at three-way replication
  - value: 38 GPUs
    label: Across seven nodes, partitioned into 74 allocatable devices
  - value: 757 users
    label: Unique interactive users over the past year, from institutions nationwide
figure:
  src: /assets/img/af-architecture.svg
  alt: "Diagram: physicists reach the Analysis Facility through ssh login nodes, dedicated Jupyter nodes or a credential-brokered MCP gateway. These feed facility services on Kubernetes, an HTCondor pool, JupyterLab and BinderHub, Coffea-casa, REANA, ServiceX, Triton inference and XCache, with 38 GPUs. Those services read and write layered storage: NFS home directories, CephFS user data, a solid-state pool, worker-local scratch, an erasure-coded cold tier and LOCALGROUPDISK at MWT2. Batch work overflows to the MWT2 pool by flocking."
  caption: Three ways in, one service layer on Kubernetes, and storage in tiers. Data paths in maroon; dashed boxes sit outside the facility itself.
links:
  - title: af.uchicago.edu
    url: https://af.uchicago.edu/
    note: The facility's front door, open to U.S. ATLAS members and their collaborators
  - title: ATLAS at MANIAC Lab
    url: /collaborations/atlas/
    note: The experiment the facility serves
  - title: Midwest Tier 2 Center
    url: /facilities/mwt2/
    note: The production site next door, which supplies overflow capacity and LOCALGROUPDISK
  - title: IRIS-HEP
    url: https://iris-hep.org/
    note: Our R&D partner; its analysis tools are developed and hardened here
  - title: Facilities and Platforms
    url: /projects/#facilities
    note: The platforms and services we develop on this infrastructure
people:
  - name: Rob Gardner
    url: /team/
    role: Principal Investigator; facility lead
  - name: Fengping Hu
    url: /team/
    role: Primary engineer of the Analysis Facility platform
  - name: Judith Stephen
    url: /team/
    role: Storage architecture and security
  - name: David Jordan
    url: /team/
    role: Systems and infrastructure
  - name: Ilija Vukotic
    url: /team/
    role: Analytics, monitoring and data delivery
  - name: Giordon Stark
    url: /team/
    role: Analysis tooling, the MCP platform and agentic interfaces to the facility
status: In production since 2021, serving U.S. ATLAS physicists at institutions across the country. Under way, a migration of user data from three-way replication to erasure coding, which roughly doubles effective capacity, along with a network refresh and consolidation of the notebook services.
---

## Getting in

Four login nodes behind a DNS round robin handle interactive sessions and job submission, with four more dedicated to Jupyter notebooks. Access is federated: a physicist signs in with their home institution's credentials rather than applying for a local account.

From there the facility offers several ways to work, and most people use more than one. A shell, for developing and compiling against software delivered over CVMFS. Notebooks, through JupyterLab and BinderHub, with Dask to scale a session out across the batch pool. Coffea-casa, a preconfigured columnar analysis environment. REANA, for workflows that have to be reproducible later. And increasingly an agentic route, where an LLM client reaches facility services through a credential-brokered gateway that mints short-lived, per-service tokens rather than handing an agent a user's credentials.

Over the past year the facility served about 458 batch users and 757 interactive users, with roughly 90 people on it in a typical day.

## Compute

Local batch is HTCondor, about 8,600 cores shared across batch submission, notebook sessions and the services that support them. When that is not enough, jobs flock to the Midwest Tier 2 Center's Chicago compute cluster, adding roughly 13,000 more cores. The two facilities share a data center and a network, so the boundary between them is close to invisible from a user's perspective.

Thirty-eight physical GPUs sit across seven nodes, spanning several NVIDIA generations and partitioned into 74 allocatable devices so that many users can share a card. Most GPU use at this facility happens inside notebooks rather than in batch jobs, which is the opposite of the pattern at most HPC centers and shapes how the capacity is carved up.

## Storage, in layers

One storage model does not fit an analysis facility. File sizes range from enormous to tiny, and access patterns from perfectly sequential to entirely random, so the facility offers several tiers and helps users pick between them:

- **Home directories** sit on an NVMe ZFS server exported over NFS, with a 100 GB quota, daily snapshots and a separate backup server. Deleted files can be recovered on request.
- **User data** lives on 5.2 PB of Ceph, about 1.7 PB usable at three-way replication, integrated into the Kubernetes cluster with Rook and mounted on every interactive and batch node. The default allocation is 5 TB per user, with a project area for groups working on shared datasets.
- **A solid-state pool** carries workflows whose input and output patterns would otherwise degrade performance for everyone on spinning disk.
- **Local scratch** on the worker nodes, a few terabytes each, handles working data that never needs to leave the machine.
- **LOCALGROUPDISK**, the Rucio storage endpoint at MWT2, holds the datasets too large for the shared filesystem, 15 TB per user by default, reachable through XRootD and local caches.

A cold tier using erasure coding has been introduced for data that is no longer actively read, returning capacity to the primary pool. Migrating the rest of the user data the same way is in progress and will roughly double effective capacity.

The traffic those tiers carry is lopsided in a useful way. Over a recent ninety-day window the shared filesystem absorbed 885 TB of writes and served 16.2 PB of reads; the facility's caches pulled 197 TB from the grid and delivered more than a petabyte from it; and the columnar delivery service read 468 TB to produce 28 TB of transformed output. Analysis is overwhelmingly a read problem, and caching and pre-transformation are what keep it affordable.

## Built on Kubernetes

Everything above the hardware runs on Kubernetes, with Ceph integrated through Rook and the cluster managed declaratively. Identity is handled by Keycloak, giving users one sign-in across the facility's web services. Batch, notebooks, data delivery, inference-as-a-service and the facility's own supporting services are all workloads on the same substrate, which is what allows a quarterly update cadence on a facility in constant use.

That choice is the facility's defining difference from the Tier 2 center beside it. MWT2 is bare metal and changes slowly by design. The Analysis Facility is expected to change, because the analysis methods it serves are still being invented.

## What it has been used to prove

The facility doubles as the proving ground for analysis at high-luminosity LHC scale, through a sequence of challenges run with [IRIS-HEP](https://iris-hep.org/), each closer to a real analysis than the last. The 2022–23 Analysis Grand Challenge ran an end-to-end top-pair analysis over 940 million events. The 2024–25 challenge pushed sustained reads past 200 Gbps, processing 32 TB across roughly 1,700 workers. The 2025–26 Integration Challenge ran a full Run-2 plus Run-3 analysis with 350 systematic variations across 765 workers, completing in about twenty minutes.

The open question those results raise is not whether the hardware can do it. It is adoption: the services behind those numbers are available to every user, and getting people onto them is the harder problem.

## The other U.S. ATLAS analysis facilities

UChicago is one of three analysis facilities serving U.S. ATLAS, alongside **Brookhaven National Laboratory**, whose facility is built on the lab's Scientific Computing and Data Facility and shared with DUNE, Belle II and FCC, and **SLAC**, whose facility runs on the Shared Science Data Facility with a Slurm batch system and a parallel filesystem.

The three have converged on a similar set of services: shell access, notebooks with scale-out, batch, and a common analysis software stack from CVMFS. They still differ in the details that matter when someone moves between them, including identity and account creation, storage quotas and capabilities, batch system and supported software environments. Reducing that friction, so an analysis written at one facility runs at another without rework, is an active concern across all three.
