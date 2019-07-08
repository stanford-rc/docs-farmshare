---
title: Software
tags: 
 - modules
 - packages
---

# Using Software on Farmshare2

So, you've logged in to FarmShare, and now you want to do something on it? We can help you with that! In addition to the software that we already have available, we have tools for you to build/install your own.

break ## The Three Software Sources

Software on FarmShare 2 comes from three sources:

 * [**Packages**](#packages), which have all of the software bits in one (or a series of) easy-to-deploy blobs, and which are installed directly on to each system.
 * [**Modules**](#modules), which either we built ourselves or we obtained from a vendor, and which are sitting on network storage that is accessible by each system.
 * [**Your own stuff**](#custom), which you have created yourself, and which typically sits in your home directory (which is on network storage), or which sits in some other common location (again, on network storage).
Of course, you probably don't care much about *what kind* of software we have, you just want to know *what* we have! Well, that's easy enough: [Go to the list of software](#list).

## Packages

Packaged software is easiest to use, because you don't have to do anything! Packaged software has already been installed on all of the systems in the environment, so to use the software, you just have to run the command.

For example, to run the packaged version of R (version 3.2.3), you just need to run the R command:

```bash
akkornel@rice03:~$ R --version R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree" 
Copyright (C) 2015 The R Foundation for Statistical Computing Platform: x86\_64-pc-linux-gnu (64-bit)  
```

FarmShare 2 runs [Ubuntu 16.04 LTS](https://wiki.ubuntu.com/XenialXerus/), which means almost anything in the [Ubuntu Xenial package repository](http://packages.ubuntu.com/xenial) is available to be installed.

## Modules

Not all software comes in the form of an Ubuntu package. In fact, as time continues to move forward, the software we do have packaged will become more and more out-of-date.

To provide up-to-date software, we use modules. Modularized software lives in shared, network-connected storage, and was either built by SRCC (like with R) or was provided a software vendor (like with SAS).

## Your Own Software

In addition to the software that we provide for you to use, it is perfectly OK for to build and use your own stuff! We support several ways that you can build random software:

* Building software from scratch.
* Using a local package manager, like [Linuxbrew](http://linuxbrew.sh) or [Spack](https://spack.io).
* We also support [Singularity](singularity.md) containers.
In addition, we have solutions for specific programming languages:

* For Perl, we have [local::lib](software-perllocallib.md) and [Perlbrew](/farmshare2/software-perlbrew).
* For Python, we have [virtualenv and conda](https://vsoch.github.io/2016/python-install/).
* For Lua, there's luarocks.

### Can You Do It For Me?

Maybe! Here are the things to think about when deciding if your software should be a module:

* *How many people are using it?* If it's just you, then it probably doesn't need to be made available to everyone. However, if this is something that is going to be used for an entirely group or class, then it's more likely to be made available.
* *Is there an existing package?* If there is an existing package repository for Xenial (that is, Ubuntu 16.04), that's much more likely to get accepted.
* *How much information do you have?* If you have an existing, easy-to-follow step-by-step guide for us, that makes things go faster, so it's more likely that your installation request will be accepted!
Either way, you should always feel free to [reach out](mailto:research-computing-support@stanford.edu?subject=FarmShare%202%20software%20installation%20request&body=Hello!%20%20I%20would%20like%20some%20new%20software%20installed%20on%20FarmShare%202.) with your request!

*A special note on upgrading:* We at SRCC have to be careful when upgrading existing software, because we don't want to break existing uses of the software. Also, if the upgrade is for a package, then we will probably have to wait for the next quarterly FarmShare downtime. Modules can normally be upgraded faster, by making a new module for the new version.

## Software Available

The table below lists all of the software packages that are installed on FarmShare 2 systems. It was last updated on Thursday, January 25, 2018 @ 9:30 AM US/Pacific time.

For software provided via a module, you will have to load the module before you can use the software! Run module avail to display a list of available software. For example, to load MATLAB R2016b, the command would be module load matlab/r2016b). Enter that command into a shell (or into a batch job script) before you use the software.

Name Packaged Version Module Version     

| Name | Packaged Version | Module Version |
|------|------------------|----------------|
| [ABINIT](http://www.abinit.org) | 7.8.2 |8.4.2 `(abinit/8.4.2)` |
| [Anaconda](https://www.continuum.io/anaconda-overview) |  | 5.0.1  for Python 2 `(anaconda/5.0.1)` <br> 5.0.1 for Python 3 `(anaconda3/5.0.1)` |
| [ANSYS](http://www.ansys.com) |  18.2 | `(ansys/18.2)` |
| [autoconf](http://www.gnu.org/software/autoconf/autoconf.html) | 2.69 |     
| [automake](https://www.gnu.org/software/automake/) | 1.15.1 |    |
| [bash](https://www.gnu.org/software/bash/) | 4.3.48 (1) |  |
| [bowtie](http://bowtie-bio.sourceforge.net/bowtie2) | 2.2.6 | |
| [Caffe](http://caffe.berkeleyvision.org) | 1.0.0 RC5 | `(caffe/1.0.0-rc5)` |
| [Caffe2](http://caffe2.ai) | 0.7.0 | `(caffe2/0.7.0)` |   
| [clang](http://clang.llvm.org) | 3.8.0 | 5.0.0 `(clang/5.0.0)`|    
| [cmake](https://cmake.org/) | 3.10.2 | |
| [CPLEX](https://www.ibm.com/products/ilog-cplex-optimization-studio) | 12.7.1 | `(cplex/12.7.1)` |
| [CUDA](http://www.nvidia.com/object/cuda_home_new.html) | 9.0.176  |    |
| [cuDNN](https://developer.nvidia.com/cudnn)   | 7.0.3 | `(cudnn/7.0.3)` |
| [Cufflinks](http://cole-trapnell-lab.github.io/cufflinks)  | 2.2.1 | `(cufflinks/2.2.1)` |
| [Emacs](https://www.gnu.org/software/emacs/)| 25.3.1 | |
| [fish](https://fishshell.com) | 2.2.0 | | 
| [GAMS](https://www.gams.com) | 24.8.3 | `(gams/24.8.3)` |
| [Gaussian](http://gaussian.com)  |G16.A03 | `(guassian/g16.a03)` <br> GaussView 6 `(gaussview/6)` |
| [GNU Fortran](https://gcc.gnu.org/fortran/) | 5.5.0 | 7.1.0 `(gcc/7.1.0)` |  
| [GCC](https://gcc.gnu.org/) | 5.5.0 | 7.1.0 `(gcc/7.1.0)`  |
| [git](https://git-scm.com/) | 2.7.4 |    |
| [Go](https://golang.org/) | 1.6.2  | |
| [Gurobi](http://www.gurobi.com) | 7.0.2 | `(gurobi/7.0.2)` |
| [Haskell](https://www.haskell.org/) | 7.10.3 | |
| [stack](https://docs.haskellstack.org/en/stable/README/) | 1.5.1 | |
| [Intel Parallel Studio XE](https://software.intel.com/en-us/intel-parallel-studio-xe) | 2017 | update 2 (intel/17.0.2) |
| [Java](https://www.oracle.com/java/index.html) | 8u151 9+181 | `(openjdk/9+181)` |
| [Julia](http://julialang.org/) | 0.5.2 | 0.6.2 `(julia/0.6.2)` |  
| [libtool](https://www.gnu.org/software/libtool/) | 2.4.6 | |
| [Lua](https://www.lua.org/)  | 5.1.5 (through the lua5.1 command) <br>5.2.4 (through the lua and lua5.2 commands) <br> 5.3.1 (through the lua5.3 command) <br> [LuaRocks](https://luarocks.org/) 2.2.0 for Lua 5.1 |
LuaRocks 2.4.1 (luarocks/2.4.1) <br> For Lua 5.1 (through the luarocks command) <br> For Lua 5.2 (through the luarocks-5.2 command) <br> For Lua 5.3 (through the luarocks-5.3 command) |
| [LuaJIT](http://luajit.org/) | 2.0.5| |
| [Mathematica](http://www.wolfram.com/mathematica) |  11.2 | `(mathematica/11.2)` |
| [MATLAB](https://www.mathworks.com/products/matlab) |  | R2017b `(matlab/r2017b)`  |
| [MEEP](http://ab-initio.mit.edu/wiki/index.php/Meep) | 1.3 | |   
| [mksh](https://www.mirbsd.org/mksh.htm) | 52c | |     
| [mosh](https://mosh.org/) | 1.3.2  | |
| [nano](https://www.nano-editor.org/) | 2.5.3  | |
| [NAG](https://www.nag.com/nag-compiler) |  | 6.1 `(nag/6.1)` |
| [OCaml](https://ocaml.org)  | 4.02.3 | |
| [OPAM](https://opam.ocaml.org) | 1.2.2 | |
| [OpenFOAM](http://openfoam.org)  | 4.1 | ParaView 5.0.1 |
| [Open MPI](https://www.open-mpi.org) | 1.10.2 | 3.0.0 `(openmpi/3.0.0)`  |
| [Perl](https://www.perl.org/) | 5.22.1 | (through the perl command) |
| [Rakudo Perl 6](http://rakudo.org) | 2015.11 | (through the perl6 command) |
| [Perlbrew](https://perlbrew.pl)|  0.74 | |
| [PGI](http://www.pgroup.com) |  Community Edition 17.4 | `(pgi/17.4)` |
| [PostgreSQL](https://www.postgresql.org) |  9.5.6 | `(postgresql/9.5.6)`|
| [PG-Strom](http://strom.kaigai.gr.jp) | v1 | |
| [PyMOL](http://pymol.org) | 1.7.2.1 |    |
| [Python](https://www.python.org/) | 2.7.12 | (through the python and python2 commands) <br> 3.5.2 (through the python3 command) |
| [virtualenv](https://virtualenv.pypa.io/en/stable) | 15.1.0 | |
| [Quantum ESPRESSO](http://www.quantum-espresso.org) | 5.1 | |
| [R](https://www.r-project.org/) | 3.4.3  | | 
| [RStudio](https://www.rstudio.com/products/rstudio/) | 1.1.383 | `(rstudio/1.10.383)` |
| [Ruby](https://www.ruby-lang.org/en/) | 2.3.1p112 |  |
| [Rust](https://www.rust-lang.org/en-US/) | 1.7.0 | |
| [cargo](https://crates.io) | 0.8.0 | |
| [SAS](http://www.sas.com/en_us/software/sas9.html)  | 9.4|  `(sas/9.4)` |
| [SCons](http://www.scons.org) | 2.4.1 | |   
| [Singularity](http://singularity.lbl.gov/) | 2.4.1 | |   
| [Stata](https://www.stata.com/why-use-stata)   |15  | `(stata-se/15, stata-mp/15)` |
| [Swift](https://swift.org)  | 4.0 | `(swift/4.0)` |
| [Tcl/Tk](http://www.tcl.tk) | 8.6 |  |  
| [tcsh](http://www.tcsh.org/Welcome) | 6.18.01 | | 
| [tmux](https://tmux.github.io/) | 2.1 | |   
| [Torch](http://torch.ch/) |  7 | `(torch/7)` |
| [vim](http://www.vim.org/) | 7.4  | | 
| [zsh](http://www.zsh.org/) | 5.1.1 | |
