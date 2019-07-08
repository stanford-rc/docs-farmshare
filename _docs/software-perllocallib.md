---
title: "Perl local::lib"
tags: 
 - perl
---

# Using Perl's local::lib for Perl modules

`local::lib` lets you have your own Perl modules, while still running system Perl.

`local::lib` lets you build and install your own Perl modules, without having to build and install your own Perl. It uses the system's Perl, as well as as the system's build environment to build modules, which are then installed into your home directory.

There are two major advantages to using `local::lib`:

* You do not have to install your own Perl.
* You can install whatever modules you want, without having to rely on SRCC to build and deploy them to a system Perl (which might not be doable easily) or a Perl module.
`local::lib` can be a quick way to begin installing your own modules when the existing system Perl already meets your needs. However, there are also disadvantages to using `local::lib`:

* You are completely reliant on the system Perl, which might not be new enough for you.
* Only you have access to your Perl modules. Since other people can't access your `local::lib`, they have to get access to a system that has the necessary bits (Perl plus modules) installed already, or they have to use `local::lib` themselves.
* The modules you build are not portable. You can not take a module that you built on FarmShare and copy it to Sherlock, because different libary versions are in use. Instead, you would have to build fresh in each environment.
It is up to you to decide if `local::lib` is the right way to go, or if [SRCC should build what you want instead](software.md). Or, if you also need control over which Perl you want to use, [Perlbrew](software-perlbrew.md) might be a better option.

## local::lib Compatibility

`local::lib` does what it does using environment variable tricks. Because of that, you have to be careful when mixing `local::lib` with other tools that also modify the environment:

### local::lib and Perlbrew

Where `local::lib` only manages Perl modules, Perlbrew gives you an entire Perl environment that is your own. *However*, you must be very careful, because using `local::lib` *and* Perlbrew might cause problems. Perlbrew works by changing your $PATH environment variable to point to a local Perl installation, which is owned by you. `local::lib` also changes $PATH, as well as other Perl-related variables, so even if Perlbrew's changes take precedence, the other environment variables may cause unexpected effects.

To be safe, you should either **just use a global local::lib or just use Perlbrew, not both**. Perlbrew can also manage multiple `local::lib` installations that are specific to each Perl you build. [Read more below](#perlbrew).

### local::lib and Taint Mode

One of the environment variables set by `local::lib` is $PERL5LIB. This extends the list of places where Perl searches for modules.

When a Perl script is run in taint mode, Perl intentionally ignores the $PERL5LIB environment variable. Running a script in Taint mode can be specified by the user (by placing -T in the perl command line), but it can also be specified by the script itself. The script does have a way of honoring $PERL5LIB even in taint mode (through the [perl5lib module](https://metacpan.org/pod/perl5lib)), but that is rarely used: The perl5lib module is a module, which if installed via `local::lib`, creates a Catch-22 situation.

Perl code which uses taint mode will not be able to use modules loaded through `local::lib`.

### Software Module compatibility

NOTE: In this section, we are talking about the software modules that are loaded using the module load command. This section does *not* refer to Perl modules unless it explicitly says "Perl modules".

Your local Perl modules *might* conflict with software modules loaded using the module load command. There are several ways that conflicts can occur:

* If you use module load to load a module, that module wants a Perl module which you installed using `local::lib`, then you need to make sure `local::lib` is active in your session before you run the software.
* If you load a GCC or clang software module, before you install a Perl module with `local::lib`, and that Perl module uses compiled code, then the Perl module will be built with that newer compiler (not the system compiler used for system Perl). That should be OK, but you will want to test your newly-built Perl module without the GCC/clang software module loaded, just to be sure it works.
* if you load a software module to get access to a shared library, and then build a Perl module which uses that shared library; then every time you want to use that Perl module, you will need to load that software module first (so that the Perl module is able to find the shared library).

## Installing local::lib

This part is really easy! We have already installed the software, but first you need to make sure it works.

If you are using `local::lib` within Perlbrew, then you don't have to do anything here, so [skip down to the next section](#perlbrew). If you are *not* using Perlbrew, then you should load the `local::lib` module once, to make sure it works:

```bash
akkornel@rice05:~$ perl -Mlocal::lib
Attempting to create directory /home/akkornel/perl5
PATH="/home/akkornel/perl5/bin${PATH+:}${PATH}"; export PATH;
PERL5LIB="/home/akkornel/perl5/lib/perl5${PERL5LIB+:}${PERL5LIB}"; export PERL5LIB;
PERL_LOCAL_LIB_ROOT="/home/akkornel/perl5${PERL_LOCAL_LIB_ROOT+:}${PERL_LOCAL_LIB_ROOT}"; export PERL_LOCAL_LIB_ROOT;
PERL_MB_OPT="--install_base \"/home/akkornel/perl5\""; export PERL_MB_OPT;
PERL_MM_OPT="INSTALL_BASE=/home/akkornel/perl5"; export PERL_MM_OPT;
```

The first message, "Attempting to create directory /home/akkornel/perl5", is what's important (although it will show your username, instead of "akkornel"). If you see that message, and no errors, then local::lib is ready to use!

All the files installed by `local::lib` go into ~/perl5, but it will *not* use the ~/perl5/perlbrew directory, which is used exclusively by Perlbrew.

Now that `local::lib` has finished its "installation", you need to decide how to use it:

* If you only want local::lib to be active in certain cases, read [Using local::lib manually](#useManually).
* If you want local::lib to be activated every time you start a new shell, read [Loading local::lib automatically](#useAutomatically).

### Installing local::lib with Perlbrew

If you are using Perlbrew, then no extra work is required, because Perlbrew includes the ability to create multiple `local::lib` environments inside your local Perl. Here's how to use `local::lib` with Perlbrew:

* If you don't have a local Perl built already, use the perlbrew install command to install one.
* Run perlbrew use to activate a local Perl, followed by perlbrew lib create to create the `local::lib` environment:

```bash
akkornel@rice03:~$ perlbrew list
  perl-5.24.0
akkornel@rice03:~$ perlbrew use 5.24.0
akkornel@rice03:~$ perlbrew lib create awesome
lib 'perl-5.24.0@awesome' is created.
```

* To activate a local Perl *with* a `local::lib`, run `perlbrew use version@libName`:

```bash
akkornel@rice03:~$ perlbrew list
* perl-5.24.0
  perl-5.24.0@awesome
akkornel@rice03:~$ perlbrew use 5.24.0@awesome
Attempting to create directory /home/akkornel/.perlbrew/libs/perl-5.24.0@awesome
```

Once you have run perlbrew use to activate your Perl with `local::lib`, code that you run (and modules that you install) will use (and be installed to) the separate Perlbrew `local::lib` directory that was created when you ran perlbrew use.

To switch back to your local Perl *without* the `local::lib`, use perlbrew use, but without the `local::lib` name:

```
akkornel@rice03:~$ perlbrew list
  perl-5.24.0
* perl-5.24.0@awesome
akkornel@rice03:~$ perlbrew use 5.24.0
akkornel@rice03:~$ perlbrew list
* perl-5.24.0
  perl-5.24.0@awesome
```

There are two ways to use `local::lib`:

 * You can use it on a case-by-case basis, only when you want it.
 * You can have `local::lib` run automatically, every time you use Perl.

Each method is discussed in detail more below:

### Using local::lib manually

There are two ways of activating local::lib manually:

 - You can use local::lib for a single Perl execution.
 - Use can use local::lib for a single session.

If you want to use local::lib for a single Perl execution, simply include `-Mlocal::lib` in the Perl command line, like so:

```bash
perl -Mlocal::lib my_perl_file.pl
```

If you want to use local::lib for an entire session, run this command for bash:

```bash
eval $(perl -I$HOME/perl5/lib -Mlocal::lib)
```

or run this command for csh or tcsh:

```bash
eval `perl -I$HOME/perl5/lib -Mlocal::lib=--shelltype=csh`
```

That will activate `local::lib` until you deactivate it, or until the current session ends (whichever comes first). Other sessions, even sessions running on the same machine, will not use `local::lib` (unless you manually activate it in that session).

### Loading local::lib automatically

To load `local::lib` automatically, you will need to make a change to one of the shell scripts that are executed every time you start a new session. Exactly what change to make depends on the shell you use.

If you are running bash, use this command to edit your .bashrc file:

```bash
echo 'eval "$(perl -I$HOME/perl5/lib/perl5 -Mlocal::lib)"' >> ~/.bashrc
```

If you are running csh/tcsh, run this command to edit your .cshrc file:

```bash
echo 'eval `perl -I$HOME/perl5/lib/perl5 -Mlocal::lib=--shelltype=csh`' >> ~/.cshrc
```

Once you have made the change, log out and log back in for the change to take effect.

If you ever decide to remove local::lib, simply remove the corresponding line from your .bashrc or .cshrc file.

#### Deactivating local::lib temporarily

If `local::lib` is loaded automatically, and you would like to deactivate it temporarily, the following line of Perl code will do that in bash:

```bash
eval $(perl -Mlocal::lib=--deactivate)
```

and the following line for csh or tcsh:

```bash
eval `perl -Mlocal::lib=--shelltype=csh,--deactivate` local::lib 
```

will remain deactivated for the current session only, and will come back the next time you log in (or start a new shell).

## Installing Perl Modules

Now that you have `local::lib` working, you probably want to install modules. When `local::lib` is active, installing modules is as simple as running the cpan command:

```bash
akkornel@rice05:~$ eval $(perl -I$HOME/perl5/lib -Mlocal::lib)
akkornel@rice05:~$ cpan install Readonly
Loading internal null logger. Install Log::Log4perl for logging messages
Reading '/home/akkornel/.cpan/Metadata'
 Database was generated on Tue, 18 Oct 2016 16:29:02 GMT

Running install for module 'Readonly'
... lots of lines snipped ...
Installing /home/akkornel/perl5/lib/perl5/Readonly.pm
Installing /home/akkornel/perl5/man/man3/Readonly.3pm
 SANKO/Readonly-2.05.tar.gz
 ./Build install  -- OK
```

In the above example, the Readonly module had several required packages that were not installed, so the cpan command downloaded and installed those as well. The system's Perl, compiler, and libraries were used for the build, and the modules you build will only be used when `local::lib` is active. Also, in the session used to make the above example, `local::lib` was not already loaded, so the appropriate command had to be run in order to load it.

One issue you may encounter, when building your own modules, is missing header files. For example, the first time cpan install Crypt::SSLeay was run on FarmShare 2, it failed to build:

```bash
akkornel@rice05:~$ eval $(perl -I$HOME/perl5/lib -Mlocal::lib)
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

If you are trying to use cpan , and it fails, it may be due to missing libraries or missing header files. Feel free to [contact us](#support) for help!

## Getting Support

SRCC is happy to help you! To get support, all you need to do is [email us](mailto:research-computing-support@stanford.edu?subject=FarmShare%202%20Perlbrew%20question). When you email us, be sure to include the following information:

* The name of the system you were on when you had problems.
* The output of the module list, env, and perl -V commands.
It would be nice if you could attach the output of those commands as separate files, instead of pasting it directly in the email!
* What you are trying to do.
* The *complete* output that you got when you ran your scripts/commands.
It would be nice if you could attach this as a separate file, instead of pasting it directly in the email!
If the problem is a missing library or missing header files, we can normally address that quickly. If the problem is an out-of-date library, that will take longer to address.

Good luck!
