---
layout: project_page
title: CI Connect
group_title: Facilities and Platforms
tagline: One front door to high-throughput computing, for a dozen research communities
lead: Since August 2013 MANIAC Lab has run hosted HTCondor access points that let a researcher sign in with a campus identity, join a project and submit work to the Open Science Pool, to campus clusters and to collaboration resources. OSG Connect was the first such front door to the Open Science Grid for individual investigators and small campus labs. The same pattern, generalized as CI Connect, carried CMS Connect for a decade and still runs SPT Connect and PSD Connect today.
facts:
  - value: 2.5 billion core hours
    label: Delivered to OSG Connect projects between 2013 and 2025, by OSG accounting
  - value: More than 650 projects
    label: From more than 250 institutions in 17 fields of science
  - value: August 2013
    label: OSG Connect and Duke Connect launched at a workshop at Duke University
  - value: Two instances live
    label: SPT Connect for the South Pole Telescope and PSD Connect for the Physical Sciences Division
figure:
  src: /assets/img/ci-connect-architecture.svg
  alt: "Diagram: researchers sign in with a campus or Globus identity to the CI Connect platform, which manages projects and memberships and provisions accounts on per-community HTCondor access points (OSG Connect, CMS Connect, ATLAS Connect, Duke Connect, Snowmass Connect, SPT Connect, PSD Connect). Jobs flow from the access points to the Open Science Pool, the Pile cluster, the Midwest Tier2 Center, and SPT storage and analysis servers, with software over CVMFS and data over the Open Science Data Federation. MWT2 and Pile also contribute opportunistic cycles to the Open Science Pool."
  caption: One platform, many front doors. Each community gets its own access point; identity, projects and accounts are shared. Retired instances are shown in grey.
links:
  - title: ci-connect.net
    url: https://www.ci-connect.net/
    note: The platform portal, with links to every Connect instance
  - title: PSD Connect
    url: https://psdconnect.uchicago.edu/
    note: Sign up here if you are a researcher in the Physical Sciences Division
  - title: SPT Connect
    url: https://spt.ci-connect.net/
    note: Access point for the South Pole Telescope collaboration
  - title: OSG Connect usage, 2013 to today
    url: https://gracc.opensciencegrid.org/d/000000129/osg-connect-community?orgId=1&from=now-20y&to=now
    note: GRACC accounting dashboard for the OSG Connect community
  - title: PATh
    url: https://path-cc.io/
    note: The Partnership to Advance Throughput Computing, which now runs the Open Science Pool access points
  - title: SPT-3G Computing (CHEP 2018)
    url: https://doi.org/10.1051/epjconf/201921403051
    note: Riedel, Bryant, Carlstrom, Crawford, Gardner et al., EPJ Web of Conferences 214, 03051 (2019)
  - title: Connective services accelerating research (GlobusWorld 2014)
    url: https://www.globusworld.org/files/2014/07-connective-services-accelerating-research-gardner.pdf
    note: Rob Gardner's talk on the OSG Connect design in its first year
people:
  - name: Rob Gardner
    url: /team/
    role: Founder and lead, 2013 to present
  - name: Lincoln Bryant
    url: /team/#former
    role: Architect and lead engineer of the platform and its access points, 2013 to 2025
  - name: Suchandra Thapa
    url: /team/#former
    role: OSG Connect software, user support and documentation in the early years
  - name: David Champion
    role: Early engineering, including the campus client that let campus clusters join the pool
  - name: Judith Stephen
    url: /team/
    role: South Pole and Chicago infrastructure for SPT since 2016; Pile and PSD Connect operations today
  - name: Benedikt Riedel
    url: /team/#former
    role: SPT-3G distributed computing and the move of SPT workflows onto the Open Science Grid, 2016 to 2018
  - name: Pascal Paschos
    url: /team/#former
    role: User support across the Connect instances and the Snowmass Connect service, 2018 to 2026
  - name: David Jordan
    url: /team/
    role: Pile cluster hardware and networking in the Hinds data center
status: SPT Connect and PSD Connect are in operation. OSG Connect ran from August 2013 through 2025, when the Open Science Pool access points passed to the OSG and PATh team. CMS Connect was hosted at UChicago for a decade and handed to Wisconsin in June 2025. ATLAS Connect was absorbed into the Analysis Facility. Duke Connect and Snowmass Connect are retired.
---

## Why a front door

In 2013 the Open Science Grid was built for large virtual organizations. An LHC experiment had the people to run submit hosts, manage certificates and negotiate with sites. A single investigator with a good high-throughput problem, or a small lab on a campus without a grid team, did not. OSG Connect closed that gap. A researcher signed in with a campus identity, joined a project, and landed on a login node with HTCondor already pointed at the pool, with software and data services ready. The OSG became, as we put it at the time, a virtual campus cluster.

OSG Connect and Duke Connect launched together in August 2013 at a workshop at Duke University. Duke Connect showed the second half of the idea: the same service could face a campus, bridging its researchers to its own cluster and to the national pool. We generalized the pattern as CI Connect, and over the following decade stood up one front door after another: CMS Connect for the U.S. CMS collaboration, ATLAS Connect for U.S. ATLAS physicists, SPT Connect for the South Pole Telescope, Snowmass Connect for the 2021 community planning exercise, and PSD Connect for the University of Chicago's Physical Sciences Division.

## What it did for the Open Science Grid

OSG Connect was the first interface to the OSG for individual investigators and small campus labs, as opposed to large virtual organizations. Between August 2013 and the end of 2025 its projects consumed about 2.5 billion core hours on the Open Science Pool, from more than 650 research projects at more than 250 institutions across 17 fields of science, by the OSG's own accounting. The service also set the pattern for how the OSG engaged campuses: most of the campuses later signed by the Partnership to Advance Throughput Computing first came to the OSG through OSG Connect.

The Lab's own facilities were on the supply side too. The Midwest Tier2 Center and, more recently, the Pile cluster contribute opportunistic cycles to the Open Science Pool, so the Lab both opened the door and helped fill the room behind it.

## How it works

Identity comes from Globus Auth, so a researcher signs in with a campus login or another federated identity and never handles a grid certificate. The CI Connect platform keeps the projects, memberships and group hierarchy and provisions Unix accounts on the access points, with Keycloak bridging those memberships to modern web applications such as JupyterHub. Each community has its own HTCondor access point. Software reaches the pool over CVMFS and data over the Open Science Data Federation, with Stash and later OSDF caches at UChicago. The same platform, with a different front page and a different resource behind it, becomes OSG Connect, SPT Connect or PSD Connect.

## SPT: a cluster at the Pole and a decade of support

The engagement with John Carlstrom's South Pole Telescope group began in April 2016, when SPT-3G, a new camera with ten times the detector count of its predecessor, was about to be installed and the collaboration needed a new computing model at the Pole and in Chicago. Over that year the Lab designed the Pole online system with the collaboration, procured and built it, and in January and February 2017 Judith Stephen deployed it on site: a hypervisor running the station's DNS, monitoring, configuration management and wikis, a small HTCondor pool for on-site analysis, and ZFS storage enclosures that hold a season of raw data for the flight north. She has returned for maintenance seasons since, and trains the winterover crew each year.

Benedikt Riedel, who had come to the Lab from IceCube, led the move of SPT-3G simulation and processing onto the Open Science Grid, work reported at CHEP 2018. In Chicago the Lab built and still runs the collaboration's analysis servers, named Scott and Amundsen, with HTCondor submission, JupyterHub, dCache and ZFS storage, a CVMFS software repository, the nightly satellite data ingest, and replication of the archive to NERSC tape. Judith supports that infrastructure in the Hinds data center and the software environment on those servers today, and SPT data now also has a home on SHARED, the campus research storage platform the Lab helped bring to UChicago with the Research Computing Center.

## PSD Connect: for anyone in the Physical Sciences Division

PSD Connect is the campus instance. It is open to any researcher in the Division, and since 2024 it connects to the Pile cluster, a Kubernetes cluster in the Hinds data center with HTCondor batch, notebook sessions through BinderHub and Ceph scratch storage, with overflow to the Open Science Pool for work that fits. A major software refresh of Pile in summer 2026, driven by the needs of the SBND collaboration, brought the stack up to date. To get started, sign in at psdconnect.uchicago.edu with your CNetID, request membership in the PSD project, and you will have a login node and a batch queue within a day. The Lab's staff provide the user support.

## Where it is going

The platform is mature and the retired instances tell the story of a job done: OSG Connect's role is now carried by the PATh team's own access points, CMS Connect moved to Wisconsin in June 2025 after a decade at UChicago, and ATLAS Connect became part of the Analysis Facility. What remains, SPT Connect and PSD Connect, is where the Lab's current work on agentic operations and the Research Platform meets a community of users on campus. The lessons of CI Connect, that identity and project membership should be shared while front doors stay specific, run through the AF MCP Platform and the Open Data Facility.
