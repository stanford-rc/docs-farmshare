---
title: About
permalink: /about/
---

# About

FarmShare 2 is the latest iteration of the FarmShare computing environment. FarmShare evolved from the old, public UNIX cluster, once located on the second floor of Sweet Hall, which was itself a successor to systems like the University's original timeshare service, LOTS. FarmShare 2 came online in Autumn Quarter 2016.

## Cluster Components

FarmShare 2 consists of three classes of servers:

* The *rice* servers are login nodes, where you log in to run commands, access files, submit jobs, and review results. The *rice*servers also have access to [Stanford AFS](https://uit.stanford.edu/service/afs). These servers can be used for interactive work, but some resource limits are enforced, so if you need to run a long-running or compute- and/or memory-intensive process you should submit a job. Remember, these are servers for research and academic use, not for administrative or business functions.
* The *wheat* servers are compute nodes. They have more CPU power and more memory than the rice servers, and are meant for both interactive jobs (where you log in to control what happens) and batch jobs (where everything runs from a script that you submit). Some *wheat* nodes also have significantly more memory than others. Like the rice servers, these are available for use for coursework or research purposes.
* The *oat* servers are GPU compute nodes. They are similar to the *wheat*nodes, except that they also have GPUs installed. These nodes are meant for computational academic or research work that is able to take advantage of a GPU (*e.g.*, TensorFlow jobs fit into this category).
All FarmShare 2 servers run [Ubuntu 16.04 LTS](https://wiki.ubuntu.com/XenialXerus/ReleaseNotes) 64-bit (x86-64 architecture).

As of June 2017, FarmShare 2 currently has 14 *rice* servers, 17 *wheat* servers (including 2 with additional memory), and 10 *oat* servers (each with a single GPU).

## Support

If you need help, please don't hesitate to [open an issue](https://www.github.com/{{ site.github_repo }}/{{ site.github_user }}).

