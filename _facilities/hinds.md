---
layout: facility_page
title: Hinds Data Center
group_title: Platforms and Services
order: 3
icon: world
tagline: The R&D platform, the collaboration clusters and the services layer
summary: Three Kubernetes clusters in the Hinds building, where new analysis patterns are proven, where Physical Sciences Division and neutrino physicists compute, and where the identity, registry and data management services other facilities depend on actually run.
lead: Hinds is not a third production site. It is where the work that is not yet production happens, and where the shared services sit that everything else leans on. Three distinct Kubernetes clusters live here, each with its own job, together accounting for more than three hundred machines across sixteen racks.
facts:
  - value: River
    label: The IRIS-HEP Scalable Systems Laboratory testbed, about 70 nodes
  - value: Pile
    label: PSD and SBND analysis cluster, with batch, notebooks and Ceph storage
  - value: Tempest
    label: The services cluster carrying identity, registry, OSG and data management
  - value: 16 racks
    label: Roughly 300 machines in the Hinds building at the University of Chicago
links:
  - title: PSD Connect
    url: https://psdconnect.uchicago.edu/
    note: Sign-in for any researcher in the UChicago Physical Sciences Division
  - title: IRIS-HEP
    url: https://iris-hep.org/
    note: The institute whose Scalable Systems Laboratory the Lab hosts
  - title: SBND
    url: /collaborations/sbnd/
    note: The neutrino collaboration Pile serves
  - title: XENON
    url: /collaborations/xenon/
    note: The dark matter experiment whose global data management runs here
  - title: South Pole Telescope
    url: /collaborations/spt/
    note: Data transfer services for SPT run here too
people:
  - name: Fengping Hu
    url: /team/
    role: Scalable Systems Laboratory and Kubernetes platform engineering
  - name: Giordon Stark
    url: /team/
    role: Scalable Systems Laboratory; analysis tooling and agentic interfaces on the River testbed
  - name: Judith Stephen
    url: /team/
    role: Storage, OpenStack and security
  - name: David Jordan
    url: /team/
    role: Cluster hardware, networking and operations
  - name: Ilija Vukotic
    url: /team/
    role: Data delivery, caching and analytics research
status: All three clusters are in production. Pile had a major software refresh in summer 2026, driven by the SBND collaboration's needs, which brought its whole stack current.
---

## River, the testbed

River is the Lab's instance of the [IRIS-HEP](https://iris-hep.org/) Scalable Systems Laboratory, the place where analysis software for the high-luminosity LHC era is run at realistic scale before it reaches a production facility. About seventy nodes, Kubernetes throughout, with Ceph storage integrated into the cluster.

What runs there is a catalogue of what the field is trying to build. ServiceX, in production and several parallel test instances, for columnar data delivery. Coffea-casa and BinderHub for interactive analysis. REANA for reproducible workflows. Dask and Parsl for scale-out. Caching proxies under test for wide-area data access. A machine learning portal and GPU-backed inference services. The perfSONAR analytics platform that archives worldwide network measurements.

River also carries infrastructure that serves the rest of the Lab: the collectors that feed the [operations analytics platform](/projects/), and a development instance of the Analysis Facility where changes are tried before they reach the physicists using the real one.

The point of River is that things are allowed to break there.

## Pile, the PSD cluster

Pile serves people, not grid workflows. It provides interactive login nodes, HTCondor batch, notebook sessions through BinderHub and JupyterHub, Ceph storage with a separate pool for scratch, CVMFS for software delivery, and a Globus endpoint for moving datasets in and out.

Two communities use it. The University of Chicago group on [SBND](/collaborations/sbnd/), the Short-Baseline Near Detector at Fermilab, has its analysis computing here, including transfers of detector data from Fermilab. And since 2024, PSD Connect has connected any researcher in the University's Physical Sciences Division to the same cluster, with a login node and a batch queue available within about a day of signing up.

Work that fits the high-throughput pattern can overflow from Pile to the Open Science Pool. Pile returns the favor, contributing its idle cycles back to the national pool.

## Tempest, the services layer

Tempest is the smallest of the three and the one everything else depends on. It runs on hardware procured through the Lab's participation in the PATh project, and it carries the services that are not tied to any single facility:

- **Identity**, the Keycloak single sign-on that users meet when they reach any of the Lab's web services
- **A container registry**, where the images the other clusters run are stored
- **OSG compute entrypoints** and the glideinWMS factory that places work onto the Open Science Pool
- **Rucio for XENON**, the global data management service the Lab operates for the dark matter experiment at Gran Sasso
- **Data transfer services for the South Pole Telescope**

It is a small cluster doing load-bearing work for several collaborations at once, which is the argument for keeping it separate from any of them.
