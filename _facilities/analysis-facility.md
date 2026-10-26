---
layout: facility_page
title: UChicago Analysis Facility
group_title: Analysis Computing
order: 2
icon: laptop
tagline: Interactive analysis at scale for U.S. ATLAS
summary: In production since 2021, the Analysis Facility gives ATLAS physicists login nodes, notebooks, batch computing, GPUs and petabytes of storage, built on Kubernetes and designed around how people actually work.
lead: The Analysis Facility was built for a different problem than a grid site. Physicists do not submit an analysis and come back in a week; they iterate, interactively, on data they want close at hand. The facility is designed around that, with user interactivity, newer analysis workflows and emerging technologies as the organizing principles, and Kubernetes as the substrate that makes change cheap.
facts:
  - value: 8,600 cores
    label: Local batch capacity, with 13,000 more reachable by flocking to MWT2
  - value: 5.2 PB
    label: Ceph-backed user storage, plus NVMe tiers for high-throughput work
  - value: 757 users
    label: Unique interactive users over the past year, from institutions nationwide
  - value: Seven GPU nodes
    label: A range of NVIDIA generations, for both interactive and batch use
links:
  - title: ATLAS at MANIAC Lab
    url: /collaborations/atlas/
    note: The experiment the facility serves
  - title: Midwest Tier 2 Center
    url: /facilities/mwt2/
    note: The production site next door, which supplies overflow capacity and LOCALGROUPDISK
  - title: IRIS-HEP
    url: https://iris-hep.org/
    note: The institute whose analysis tools are developed and hardened here
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
status: In production since 2021, serving U.S. ATLAS physicists at institutions across the country. Under way, a migration of user data from three-way replication to erasure coding, which roughly doubles effective capacity, along with a network refresh and consolidation of the notebook services.
---

## Getting in

Four login nodes behind a DNS round robin handle interactive sessions and job submission, with four more dedicated to Jupyter notebooks. From there a physicist can work at a shell, open a notebook, submit batch work, or do all three against the same data.

The notebook environment is the part that has grown most. In-house JupyterLab, coffea-casa and BinderHub all run here, with Dask for scale-out and submission into the local batch system from inside a notebook. ServiceX delivers columnar data; REANA runs reproducible analysis workflows. Over the past year the facility has served about 458 batch users and 757 interactive users, with roughly 90 people using it on a typical day.

## Compute

Local batch is HTCondor, about 8,600 cores shared across batch submission, notebook sessions and the services that support them. When that is not enough, jobs flock to the Midwest Tier 2 Center's Chicago compute cluster, adding roughly 13,000 more cores. The two facilities share a data center and a network, so the boundary between them is close to invisible from a user's perspective.

Seven GPU nodes span several NVIDIA generations and are available both interactively and through batch, partitioned so that many users can share a physical card.

## Storage, in layers

One storage model does not fit an analysis facility. File sizes range from enormous to tiny, and access patterns from perfectly sequential to entirely random, so the facility offers several tiers and helps users pick between them:

- **Home directories** sit on an NVMe ZFS server exported over NFS, with daily snapshots synchronized to a separate backup server. Deleted files can be recovered on request.
- **User data** lives on 5.2 PB of Ceph, integrated into the Kubernetes cluster with Rook and mounted on every interactive and batch node. A project area serves groups working on shared datasets.
- **A solid-state pool** carries workflows whose input and output patterns would otherwise degrade performance for everyone on spinning disk.
- **Local scratch** on the login and batch nodes handles working data that never needs to leave the machine.
- **CERN EOS** can be mounted directly on the login nodes with a physicist's CERN credentials, so data already at CERN is reachable without a separate transfer.
- **LOCALGROUPDISK**, the Rucio endpoint at MWT2, holds the datasets too large for the shared filesystem, reachable through XRootD and local caches.

A cold tier using erasure coding has been introduced for data that is no longer actively read, returning capacity to the primary pool. Migrating the rest of the user data the same way is in progress and will roughly double effective capacity.

## Built on Kubernetes

Everything above the hardware runs on Kubernetes, with Ceph integrated through Rook and the cluster managed declaratively. Identity is handled by Keycloak, which gives users one sign-in across the facility's web services. Batch, notebooks, data delivery and the facility's own supporting services are all workloads on the same substrate, which is what allows a quarterly update cadence on a facility in constant use.

That choice is the facility's defining difference from the Tier 2 center beside it. MWT2 is bare metal and changes slowly by design. The Analysis Facility is expected to change, because the analysis methods it serves are still being invented.
