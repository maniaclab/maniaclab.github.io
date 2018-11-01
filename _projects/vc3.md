---
layout: post
title: VC3
categories: [projects]
author: Jeremy Van
icon: /assets/projects/vc3-logo.png
excerpt: Virtual Clusters for Community Computation
---

## The Cluster Re-Imagined

The virtual clusters for community computation (VC3) project aims to make
research computing facilities more accessible to use by the broader user communities.
In the VC3 paradigm, the end user "allocates" a virtual cluster (VC) from
existing facilities by requesting, for example, 200 nodes of 24 cores and 64GB
RAM each, with 100TB local scratch and 10Gb connectivity to external data sources.
Once allocated, the owner of the virtual cluster installs the required software,
transfers input data, and executes jobs. The users can share computing resources
with collaborators using the VC. We do not intend to change the resource
management systems or infrastructure of existing facilities.
Rather, VCs will be provisioned on top of existing facilities by deploying
user-level tools (e.g. HTCondor-based tools for resource management,
data caching, and Parrot for virtual file system access) within the existing
batch systems. In short, a VC will appear as a large parallel job managed by
the end user to the facility administrators .

## Smarter workloads

Virtual clusters by themselves are not enough: workloads must be self-configuring
so that they can bring necessary dependencies, discover the dynamic configuration,
and adapt to the (possibly) changing conditions of the virtual cluster. To
accomplish this integration, we will leverage a variety of existing open-source
tools and production-tested services. The end product will be a flexible,
optimizable software ecosystem with a generic set of capabilities, presented to
users as a computing resources aggregation service accessible via a web portal.
We presume the cluster as an optimal abstraction as it naturally covers a large
swath of modern scientific computation and training base of expertise (e.g.
campus HPC centers). Our principal drivers are computing requirements in
high-energy physics, and in particular analysis workflows requiring using
multi-petabyte scale datasets from the ATLAS and CMS experiments at the
Large Hadron Collider (LHC) at CERN in Geneva, Switzerland.
