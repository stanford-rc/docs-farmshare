---
title: Perlbrew
tags: 
 - perl
---

# Using Perlbrew for custom Perl environments

Perlbrew lets you have your own complete Perl installation, while leveraging the system's libraries and toolchain.

Perlbrew is a Perl module that orchestrates the building of Perls within your home directory, and modifying your session's environment so that you can use your local Perl automatically. Almost all major versions of Perl can be built using Perlbrew, and Perl code run by your local Perl does not know (or care) that you are running from a local Perl (as opposed to the system Perl).

There are two major advantages to using Perlbrew:

* You can have the latest released version of Perl, without having to wait for SRCC to build and install it.
* You can install whatever modules you want, without having to rely on SRCC to build and deploy them to a system Perl (which might not be doable easily) or a Perl module.
Perlbrew is by far the best option when you are doing any sort of Perl development, because it is so easy to blow away and rebuild the environment when something goes wrong. However, there are also disadvantages to using Perlbrew:

* Only you have access to your Perlbrew. Since other people can't access it directly, they have to get access to a system that has the necessary bits (Perl plus modules) installed already, or they have to make their own Perlbrew.
* Each Perlbrew is not portable. You can not take a Perl that you built on FarmShare and copy it to Sherlock, because different libary versions are in use. Instead, you would have to build fresh in each environment.
* Each Perl takes space in your home directory. For example, Perl 5.24 with a number of modules takes ~725 MB of space.
It is up to you to decide if Perlbrew is the right way to go, or it [SRCC should build it for you](software.md) as a module. Another alternative, if you just want a module and are OK with using the system Perl, is to [use local::lib](software-perllocallib.md).

## Installing Perlbrew

This part is really easy! You don't need to install anything (we did that already), *but* you do need to initialize Perlbrew. You only have to do this once, and it's easy:

```bash
akkornel@rice05:~$ perlbrew init

perlbrew root (~/perl5/perlbrew) is initialized.

Append the following piece of code to the end of your ~/.bash_profile and start a
new shell, perlbrew should be up and fully functional from there:

    source ~/perl5/perlbrew/etc/bashrc

Simply run `perlbrew` for usage details.

Happy brewing!
```

You might be told to add something to your `~/.bash_profile` file, or you might be told to edit a different file (it depends on what shell you are using). Once that is done, log out and log back in.

For reference, Perlbrew installs all of its software at the path `~/perl5/perlbrew`. If you really want to get rid of everything, simply delete that directory, undo the change to your shell profile, log out, and log back in.

## Building a Perl

Once Perlbrew has been configured, you can go ahead and install Perl! Perlbrew has Perls going back as far as 5.4.5; new Perls become available once they are released.

First, find out what Perl versions are available:

```
akkornel@rice05:~$ perlbrew available
  perl-5.24.0
  perl-5.22.2
  perl-5.20.3
... there are lots more!
```

Once you have chosen the Perl you want to install, simply run perlbrew install with the Perl version you want. For example, perlbrew install perl-5.24.0. Or, to get the latest stable Perl, you can run perlbrew install stable, like so:

```
akkornel@rice05:~$ perlbrew --notest install stable
Installing /home/akkornel/perl5/perlbrew/build/perl-5.24.0 into ~/perl5/perlbrew/perls/perl-5.24.0

This could take a while. You can run the following command on another shell to track the status:

  tail -f ~/perl5/perlbrew/build.perl-5.24.0.log

perl-5.24.0 is successfully installed.
```

The `--notest` option is used to disable all of the post-build tests. This is needed because one of the tests (the filesystem access-time test) fails (because we don't track last-accessed time on network storage). This is expected by us, but Perlbrew expects all tests to pass, so `--notest` is used to disable post-build testing.

You should expect the entire build process to take about half an hour. Once built, your Perl will remain available until you change or delete it.

## Using Your Perl

Once Perl is installed, you can use it on any of the systems in the cluster. To see what versions are available, run perlbrew list:

```
$ akkornel@rice05:~$ perlbrew list
  perl-5.24.0
```

To actually use a particular Perl, the command is `perlbrew use`:

```
akkornel@rice05:~$ perl -v | grep version
This is perl 5, version 22, subversion 1 (v5.22.1) built for x86_64-linux-gnu-thread-multi
akkornel@rice05:~$ perlbrew list
  perl-5.24.0
akkornel@rice05:~$ perlbrew use 5.24

ERROR: The installation "5.24" is unknown.

akkornel@rice05:~$ perlbrew use 5.24.0
akkornel@rice05:~$ perl -v | grep version
This is perl 5, version 24, subversion 0 (v5.24.0) built for x86_64-linux
```

The perlbrew use command can also be placed into scripts, so that your batch jobs can use the Perl you built. You can also leave out the "perl-" prefix when you run perlbrew use.

### Software Module compatibility

NOTE: In this section, we are talking about the software modules that are loaded using the module load command. This section does *not* refer to Perl modules, unless it explciitly says "Perl module".

Your local Perls *might* conflict with software modules loaded using the module load command. There are several ways that conflicts can occur:

* If you run perlbrew use, and *then* use module load to load a module, and *that* module uses Perl, then that module might end up using your local Perl. This is normally OK, *as long as* your local Perl has all of the Perl modules that the software wants to use.
* If you load a GCC or clang module, before you run perlbrew install, then your Perl will be built with that newer compiler. That should be OK, but you will want to test your newly-built Perl without the GCC/clang module loaded.
* if you load a software module to get access to a shared library, and then build a Perl module which uses that shared library; then every time you want to use that Perl module, you will need to load that software module first (so that the Perl module is able to find the shared library).
The important thing to remember is, in most cases, the later software is loaded, the higher its precedence. If you run module load perl/5.24.0, and then run perlbrew use 5.22.2, then you will *probably *be using Perl 5.22 (running perl -v will tell you for sure).

## Installing Perl Modules

Now that you have a Perl built, you probably want to install modules. Installing modules is easy enough with Perlbrew: First you run perlbrew use to load the Perl version that should get the modules, then you run cpan install Module\_Name, which will find the latest version of the module, build it, test it, and (assuming all those steps passed) install it:

```
akkornel@rice05:~$ perlbrew list
  perl-5.24.0
akkornel@rice05:~$ perlbrew use 5.24.0
akkornel@rice05:~$ cpan install Crypt::SSLeay
Loading internal null logger. Install Log::Log4perl for logging messages
Reading '/home/akkornel/.cpan/Metadata'
  Database was generated on Tue, 18 Oct 2016 16:29:02 GMT
Running install for module 'Crypt::SSLeay'
... many, many lines of output skipped here ...
Appending installation info to /home/akkornel/perl5/perlbrew/perls/perl-5.24.0/lib/5.24.0/x86_64-linux/perllocal.pod
  NANIS/Crypt-SSLeay-0.72.tar.gz
  /usr/bin/make install  -- OK
```

In the above example, the Crypt::SSLeay module had several required packages that were not installed, so cpan downloaded and installed those as well. The system's compiler, and system libraries, were used for the build, but all Perl code was executed by your local Perl (*not* system Perl), and the modules you build will only be used by your local Perl (specifically, the one you're using to run the cpan command).

One issue you may encounter, when building your own modules, is missing header files. For example, the first time cpan install Crypt::SSLeay was run on FarmShare 2, it failed to build:

```
akkornel@rice05:~$ perlbrew use 5.24.0
akkornel@rice05:~$ cpan install Crypt::SSLeay
Loading internal null logger. Install Log::Log4perl for logging messages
Reading '/home/akkornel/.cpan/Metadata'
  Database was generated on Tue, 18 Oct 2016 16:29:02 GMT
Running install for module 'Crypt::SSLeay'
... many, many lines of output skipped here ...
openssl-version.c:2:30: fatal error: openssl/opensslv.h: No such file or directory
compilation terminated.
Failed to build and link a simple executable using OpenSSL
No 'Makefile' created  NANIS/Crypt-SSLeay-0.72.tar.gz
  /home/akkornel/perl5/perlbrew/perls/perl-5.24.0/bin/perl Makefile.PL -- NOT OK
akkornel@rice05:~$
```
This failure happened because a package was missing: Crypt::SSLeay uses OpenSSL to do the actual work; although the OpenSSL libraries were installed, the OpenSSL header files were not. Header files are only needed when you are building software, so the lack of header files caused the build to fail. In this case, installing the libssl-dev package was enough to solve the problem.

If you are trying to use cpan in your own Perl, and cpan fails, it may be due to missing libraries or missing header files. Feel free to [contact us](#support) for help!

## Other Commands

In addition to the commands we've explored so far, there are other helpful commands available:

* perlbrew off turns off Perlbrew for the current shell. In other words, this switches you back to the Perl version that is installed on the system.
* perlbrew upgrade-perl will take an already-installed Perl, and upgrade it. For example, if you used perlbrew install perl-5.22.1 to build and install Perl 5.22.1, you could upgrade that by running perlbrew use 5.22.1, followed by perlbrew upgrade-perl.
* perlbrew switch makes a specific Perl the defaut one for all future sessions. So, if you have already built Perl 5.24.0 (using perlbrew install perl-5.24.0), then running perlbrew switch 5.24.0 will make Perl 5.24.0 available every time you start a new session. To un-do this command, run perlbrew switch-off.

## Getting Support

SRCC is happy to help you! To get support, all you need to do is [email us](mailto:research-computing-support@stanford.edu?subject=FarmShare%202%20Perlbrew%20question). When you email us, be sure to include the following information:

* The name of the system you were on when you had problems.
* The output of the perlbrew list and perl -V commands.
It would be nice if you could attach the output of those commands as a separate file, instead of pasting it directly in the email!
* What you are trying to do.
* The *complete* output that you got when you ran your scripts/commands.
It would be nice if you could attach this as a separate file, instead of pasting it directly in the email!
If the problem is a missing library or missing header files, we can normally address that quickly. If the problem is an out-of-date library, that will take longer to address.

Good luck!
