---
layout: facility_page
title: Midwest Tier 2 Center
group_title: Production Computing
order: 1
icon: server
tagline: ATLAS production computing on the Worldwide LHC Computing Grid
summary: A U.S. ATLAS Tier 2 center since 2005, run as one facility across three universities, delivering simulation, reconstruction and analysis for the ATLAS experiment at CERN.
lead: MWT2 is one of five U.S. ATLAS Tier 2 centers and among the largest ATLAS computing sites outside CERN itself. It is operated as a single facility spanning the University of Chicago, Indiana University and the University of Illinois Urbana-Champaign, with a bare-metal design that prioritizes long-term stability over novelty.
facts:
  - value: 42,000 job slots
    label: About 646,000 HEPSpec of compute, scheduled with HTCondor
  - value: 26 PB
    label: Active disk storage served by dCache at Chicago and Indiana
  - value: 99.8% reliability
    label: September 2025 to September 2026, from WLCG published records
  - value: Three universities
    label: Chicago, Indiana and Illinois Urbana-Champaign, run as one site
figure:
  src: /assets/img/mwt2-dc.jpg
  alt: "Row A in the POD-C data centre: two rack doors stand open on the left, showing columns of Dell servers with blue status lights, while a line of closed white racks recedes down the corridor under overhead cable trays and conduit."
  caption: Row A in POD-C, with the front doors of two racks open. The Analysis Facility and the UChicago half of MWT2 share this room.
links:
  - title: Worldwide LHC Computing Grid
    url: https://wlcg.web.cern.ch/
    note: The global grid MWT2 is part of
  - title: U.S. ATLAS
    url: https://www.usatlas.org/
    note: The national program the center serves
  - title: ATLAS at MANIAC Lab
    url: /collaborations/atlas/
    note: The experiment and our role in it
  - title: Operations Analytics Platform
    url: /projects/
    note: The Elasticsearch platform MWT2 hosts for U.S. ATLAS monitoring and logs
people:
  - name: Rob Gardner
    url: /team/
    role: Principal Investigator; UChicago site lead
  - name: Fred Luehring
    url: /team/
    role: Site lead, Indiana University
  - name: Judith Stephen
    url: /team/
    role: Storage, OpenStack and security
  - name: David Jordan
    url: /team/
    role: Systems and infrastructure; site reporting
  - name: Aidan Rosberg
    url: /team/
    role: Indiana site operations and heterogeneous resource integration
status: In continuous production since 2005. A network refresh is under way, taking the UChicago wide-area connection from 200 to 400 Gbps and replacing storage top-of-rack switches, alongside an operating system migration and the move to the next OSG and HTCondor release.
---

## What it does

MWT2 runs the ATLAS production grid workflow: Monte Carlo event generation and simulation, reconstruction, group production and user analysis, scheduled centrally by ATLAS and delivered continuously. Work arrives through the grid rather than through a login prompt, and the facility's job is to keep accepting it without interruption.

That shapes the design. MWT2 is deliberately bare metal and deliberately conservative, built for long-term support and stability rather than for rapid change. It is the counterweight to the Analysis Facility, which sits a few racks away in the same Chicago data center and is built on Kubernetes for exactly the opposite reason.

<figure class="project-figure">
  <img src="/assets/img/mwt2-architecture.svg" alt="Diagram: ATLAS grid services PanDA, Rucio and XRootD feed a single facility spanning the University of Chicago, Indiana University and the University of Illinois. Its compute forms one HTCondor pool of 42,000 job slots and its disk is 26 PB on dCache at Chicago and Indiana. The centre also serves the UChicago Analysis Facility with flocked cores and LOCALGROUPDISK, hosts the operations analytics platform, and contributes idle cycles to the Open Science Pool.">
  <figcaption>Work arrives from the ATLAS grid in maroon; the three university sites present as one facility. Dashed lines are services the centre provides beyond ATLAS production.</figcaption>
</figure>

## Across three universities

The center is split between the University of Chicago, Indiana University and the University of Illinois Urbana-Champaign, with the three sites presenting to ATLAS as a single resource. Compute is distributed across all three; disk storage is served by dCache at Chicago and Indiana.

The hardware spans about a decade of procurement cycles, with the compute fleet averaging roughly five years old and the storage fleet closer to three. Equipment is retired as it ages out, and the facility has twice absorbed the relocation of entire data centers without interrupting production.

## Compute and storage

Batch scheduling is HTCondor throughout. Storage is dCache, with XRootD providing the wide-area data access that lets ATLAS jobs read from the nearest copy of a dataset rather than waiting for a transfer. Data placement across the grid is managed by Rucio, the ATLAS data management system.

The facility also provides LOCALGROUPDISK, a Rucio storage endpoint where individual physicists keep datasets larger than a shared analysis filesystem can reasonably hold. Because it sits on the same networking infrastructure as the Analysis Facility, analysts can reach that data at local speeds.

## Beyond ATLAS

MWT2 hosts the bare-metal Elasticsearch cluster behind the Lab's [operations analytics platform](/projects/), which indexes operational data from across ATLAS distributed computing and serves coordinators, shifters and, increasingly, AI agents. The center also contributes opportunistic cycles to the Open Science Pool when ATLAS is not using them, so capacity procured for the LHC supports researchers well outside high energy physics.

## Reliability

ATLAS measures its sites continuously. Over the year from September 2025 to September 2026, MWT2 recorded 99.8% reliability in WLCG's published figures, while running a near-constant load of tens of thousands of simultaneous jobs.
