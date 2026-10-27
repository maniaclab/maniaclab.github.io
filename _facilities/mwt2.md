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
  - value: 42,000 cores
    label: About 21,000 physical, roughly 646,000 HS06, scheduled with HTCondor
  - value: 26 PB on dCache
    label: Around 88 million files under management, served at Chicago and Indiana
  - value: 9.2 million jobs a quarter
    label: At about 93% success, moving roughly 40 PB over the research network
  - value: Second only to CERN
    label: Among ATLAS computing centres worldwide, and the largest in the United States
figure:
  src: /assets/img/mwt2-dc.jpg
  alt: "Row A in the POD-C data centre: two rack doors stand open on the left, showing columns of Dell servers with blue status lights, while a line of closed white racks recedes down the corridor under overhead cable trays and conduit."
  caption: Row A in POD-C, with the front doors of two racks open. The Analysis Facility and the UChicago third of MWT2 share this room.
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
    role: Principal Investigator, Indiana University site
  - name: Mark Neubauer
    url: https://physics.illinois.edu/people/directory/profile/msn
    role: Principal Investigator, University of Illinois Urbana-Champaign site
  - name: Judith Stephen
    url: /team/
    role: Storage, OpenStack and security
  - name: David Jordan
    url: /team/
    role: Systems and infrastructure; site reporting
  - name: Aidan Rosberg
    url: /team/
    role: Indiana site operations and heterogeneous resource integration
status: 'In continuous production since 2005. The three sites are joined at 2x100 Gbps over ESnet and Internet2. A network refresh now under way targets the internal fabric, improving the path between storage — both MWT2 LOCALGROUPDISK and the Analysis Facility Ceph — and Analysis Facility compute. An operating system migration and the move to the next OSG and HTCondor release are in progress alongside it.'
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

The three campuses carry very nearly a third of the load each — in jobs, in compute delivered and in events processed — and are joined by a dedicated research network, a Science DMZ running at 2x100 Gbps over ESnet and Internet2, with a shared data layer that lets any site's storage and compute serve the others. That fabric is not incidental to the Lab's work: MANIAC Lab has been the primary scientific driver and funding justification for the University's campus and wide-area network upgrades across two NSF networking awards.

The hardware spans about a decade of procurement cycles, with the compute fleet averaging roughly five years old and the storage fleet closer to three. Equipment is retired as it ages out, and the facility has twice absorbed the relocation of entire data centers without interrupting production.

## The Illinois site

The Illinois third of MWT2 works differently from the other two. Rather than a cluster of its
own, it is an investment in the **Illinois Campus Cluster Program**, the university's
campus-wide high-performance computing resource. Under its model, groups and units buy compute
and storage and are guaranteed the nodes they paid for; when an owner is not using their share,
the cycles go to other Illinois researchers. MWT2 benefits from the same arrangement in
reverse. For ATLAS production, which can absorb any amount of opportunistic capacity and does
not mind waiting, that is an unusually good fit: the site gets a floor it can count on and a
ceiling it does not pay for.

The hardware lives in the **National Petascale Computing Facility**, which NCSA operates. It is
88,000 square feet, LEED Gold, and was built for Blue Waters. For much of the year three
on-site cooling towers chill water using outside air rather than drawing on the campus
chilled-water loop, and the building runs at the high end of the ASHRAE envelope, with
equipment rated for 65°F inlet water and 78°F inlet air. MWT2 is a small tenant in a national
facility, which is precisely the advantage: ATLAS gets that standard of power, cooling and
operations without having to build or run a machine room for it.

Mark Neubauer, Professor of Physics at Illinois, is the site's principal investigator.

## Compute and storage

Batch scheduling is HTCondor throughout. Storage is dCache, with XRootD providing the wide-area data access that lets ATLAS jobs read from the nearest copy of a dataset rather than waiting for a transfer. Data placement across the grid is managed by Rucio, the ATLAS data management system.

The facility also provides LOCALGROUPDISK, a Rucio storage endpoint where individual physicists keep datasets larger than a shared analysis filesystem can reasonably hold. Because it sits on the same networking infrastructure as the Analysis Facility, analysts can reach that data at local speeds.

## What a quarter looks like

In a recent quarter MWT2 completed about **9.2 million jobs** at roughly 93% success and moved
close to **40 petabytes** over the research network. It holds around 88 million files.

The wide-area traffic is a two-way exchange with the rest of the grid rather than a one-way
feed: roughly 23 PB in and 17 PB out over the quarter. The heaviest partners in both
directions are the Brookhaven Tier-1 and CERN, followed by the European and Canadian Tier-1s —
RAL, TRIUMF, IN2P3 — and the other U.S. Tier-2s. Data arrives from a site in Tokyo as readily
as from one in New York, which is what a global grid looks like in practice.

## Two roles at once

MWT2 is simultaneously a top **production** site and a heavy **analysis** endpoint, and the two
workloads look nothing alike. Managed production consumes about 71% of the compute. User
analysis accounts for 58% of the jobs and reads **94% of all events**.

That asymmetry is the real shape of the facility. Production is a smaller number of long,
compute-heavy jobs; analysis is a flood of short jobs that touch enormous amounts of data.
Designing for either one alone would get the other badly wrong, and it is why storage
throughput, not core count, is usually the thing that limits what the site can do.

## Beyond ATLAS

MWT2 hosts the bare-metal Elasticsearch cluster behind the Lab's [operations analytics platform](/projects/), which indexes operational data from across ATLAS distributed computing and serves coordinators, shifters and, increasingly, AI agents. The center also contributes opportunistic cycles to the Open Science Pool when ATLAS is not using them, so capacity procured for the LHC supports researchers well outside high energy physics.

## Reliability

ATLAS measures its sites continuously. Over the year from September 2025 to September 2026, MWT2 recorded 99.8% reliability in WLCG's published figures, while running a near-constant load of tens of thousands of simultaneous jobs.
