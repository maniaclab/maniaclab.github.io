---
layout: collaboration_post
title: Computing for the South Pole Telescope
categories: [collaborations]
author: Rob Gardner
order: 4
image: /assets/img/slideshow/spt.jpg
icon: /assets/collaborations/spt-logo-9.jpg
excerpt: Since 2016 MANIAC Lab has built and run computing for the South Pole Telescope at the Pole and in Chicago, from the SPT-3G online system Judith Stephen deployed on site to the analysis servers, storage and SPT Connect access point the collaboration uses today.
---
The South Pole Telescope (SPT) is a 10-meter millimeter-wave telescope at the Amundsen-Scott South Pole Station, led from the University of Chicago by John Carlstrom, that measures the temperature and polarization of the cosmic microwave background. MANIAC Lab's engagement began in April 2016, as the SPT-3G camera, with ten times the detectors of its predecessor, was about to be installed and the collaboration needed a new computing model at the Pole and in Chicago.

At the Pole the Lab designed, built and shipped the SPT-3G online system: a hypervisor carrying the station's DNS, monitoring, configuration management and wikis, a small HTCondor pool for on-site analysis, and ZFS storage enclosures that hold a season of raw data for the trip north. Judith Stephen deployed it on site in January and February 2017 and has returned for maintenance seasons since, training each year's winterover crew. Benedikt Riedel, who joined the Lab from IceCube, led the move of SPT-3G simulation and processing onto the Open Science Grid, work the team reported at [CHEP 2018](https://doi.org/10.1051/epjconf/201921403051).

In Chicago the Lab runs the collaboration's analysis servers, Scott and Amundsen, with HTCondor submission, JupyterHub, dCache and ZFS storage, a CVMFS software repository, the nightly ingest of satellite data from the Pole, and replication of the archive to NERSC tape. The [SPT Connect](https://spt.ci-connect.net/) access point, part of the Lab's [CI Connect](/projects/ci-connect/) platform, gives collaborators a single sign-in to all of it. Judith supports that infrastructure in the Hinds data center and the software environment on the servers today, and SPT data has a home on SHARED, the campus research storage platform the Lab helped bring to UChicago with the Research Computing Center.

(Photo by Jason Gallicchio)
