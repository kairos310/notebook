```
pip install meson-1.2.3-py3.11

linker ??
/home/zwong/autobuild/Linux-splprisn02.stjude.org-gtk4/lib/python3.11/site-packages/numpy-*/numpy/lib/_version.py

```

# README

 *Coot*
 ----

 *Coot* is a toolkit for Macromolecular Crystallography and
 model-building.  *Coot* uses widgets (with the gui builder glade),
 mmdb, clipper, and OpenGL, together with a new approach to map
 contouring and importing/creation and other modelling and building
 operations.

 Blog
 ----

 [Coot Development Blog](https://pemsley.github.io/coot/ "Coot Development Blog")


 [![Powered by RDKit](https://img.shields.io/badge/Powered%20by-RDKit-3838ff.svg?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQBAMAAADt3eJSAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6J   gAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAFVBMVEXc3NwUFP8UPP9kZP+MjP+0tP////9ZXZotAAAAAXRSTlMAQObYZgAAAAFiS0dEBmFmuH0AAAAHdElNRQfmAwsPGi+MyC9RAAAAQElEQVQI12NgQABGQUEBMENISUkRLKBsbGwEE   hIyBgJFsICLC0iIUdnExcUZwnANQWfApKCK4doRBsKtQFgKAQC5Ww1JEHSEkAAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMi0wMy0xMVQxNToyNjo0NyswMDowMDzr2J4AAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjItMDMtMTFUMTU6MjY6NDcrMDA6MDBNt   mAiAAAAAElFTkSuQmCC)](https://www.rdkit.org/)

 [![Python 3.11](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/downloads/release/python-3114/)

Basic Installation
==================

   The `configure` shell script attempts to guess correct values for
various system-dependent variables used during compilation.  It uses
those values to create a `Makefile` in each directory of the package.
It may also create one or more `.h` files containing system-dependent
definitions.  Finally, it creates a shell script `config.status` that
you can run in the future to recreate the current configuration, a file
`config.cache` that saves the results of its tests to speed up
reconfiguring, and a file `config.log` containing compiler output
(useful mainly for debugging `configure`).

   If you need to do unusual things to compile the package, please try
to figure out how `configure` could check whether to do them, and mail
diffs or instructions to the address given in the `README` so they can
be considered for the next release.  If at some point `config.cache`
contains results you don't want to keep, you may remove or edit it.

   The file `configure.in` is used to create `configure` by a program
called `autoconf`.  You only need `configure.in` if you want to change
it or regenerate `configure` using a newer version of `autoconf`.

The simplest way to compile this package is:

  1. `cd` to the directory containing the package's source code and type
     `./configure` to configure the package for your system.  If you're
     using `csh` on an old version of System V, you might need to type
     `sh ./configure` instead to prevent `csh` from trying to execute
     `configure` itself.

     Running `configure` takes awhile.  While running, it prints some
     messages telling which features it is checking for.

  2. Type `make` to compile the package.

  3. Optionally, type `make check` to run any self-tests that come with
     the package.

  4. Type `make install` to install the programs and any data files and
     documentation.

  5. You can remove the program binaries and object files from the
     source code directory by typing `make clean`.  To also remove the
     files that `configure` created (so you can compile the package for
     a different kind of computer), type `make distclean`.  There is
     also a `make maintainer-clean` target, but that is intended mainly
     for the package's developers.  If you use it, you may have to get
     all sorts of other programs in order to regenerate files that came
     with the distribution.

Compilers and Options
=====================

   Some systems require unusual options for compilation or linking that
the `configure` script does not know about.  You can give `configure`
initial values for variables by setting them in the environment.  Using
a Bourne-compatible shell, you can do that on the command line like
this:
     CC=c89 CFLAGS=-O2 LIBS=-lposix ./configure

Or on systems that have the `env` program, you can do it like this:
     env CPPFLAGS=-I/usr/local/include LDFLAGS=-s ./configure


Building Automatically
----------------------

 I`d say that the easiest way to try to build coot is to use the
 auto-builder:

 https://raw.githubusercontent.com/pemsley/coot/refinement/build-it

 It checks for dependencies and builds them if needed.

 From a fresh ubuntu install, you also need to install:

 patch
 m4
 g++
 libxext-dev
 libxt-dev
 libc6-dev
 libglu1-mesa-dev
 mesa-common-dev
 swig
 libgtk2.0-dev
 libgnomecanvas2-dev


Building by Hand
----------------

 don't do this.  Try autobuilding first.  If that fails report to Paul
 (unless building on Windows, then report to Bernhard).  If you try to
 autobuild on Macintosh OS X: good luck.

 OK, if you ignore that advice.....

 First build and install mmdb and clipper.

  mmdb
  ----

 MMDB is CCP4`s Macromolecular Coordinate Library written by Eugene
 Krissinel at the EBI.

 Note that official mmdb as distributed by Eugene does not install and
 does conform to normal "unix" standard notions of the behaviour of
 include and libraries. So either you will doing something by hand,
 and that is to link (or copy) mmdb.a to libmmdb.a (and that it is in
 the top directory of mmdb).

 Or the easier method is to pick up the version of mmdb that has been
 packaged with a GNU configure script from 

 ftp://ftp.ccp4.ac.uk/opensource/mmdb2-2.0.1.tar.gz

 is the current version (at time of writing).

  clipper
  -------

Right now, building clipper is non-trivial.  clipper depends on mmdb,
so compile mmdb first.  

ftp://ftp.ccp4.ac.uk/opensource/clipper-2.1.20140911.tar.gz

Configure clipper something like this:
```
./configure --prefix=$HOME/crystal --with-fftw=$HOME/crystal --with-mmdb=$HOME/crystal --with-ccp4=$HOME/crystal --enable-mmdb --enable-mtz --enable-cif --enable-minimol --enable-phs
```
Clipper should give a summary like this (if it does not either clipper
or coot will not work)
```
		Configuration Summary
 ------------------------------
core:          yes
contrib:       yes
phs:           yes
mmdb:          yes
mmdbold:       yes
minimol:       yes
cif:           yes
mtz:           no
ccp4:          yes
cctbx:         no



 Other dependences:
 -----------------

 glib:

   If necessary, use: --with-glib-prefix=PREFIX

 gtk:

   If necessary, use: --with-gtk-prefix=PREFIX

 glut: 

   If necessary, use: --with-glut-prefix=PREFIX

 gtkglarea:

   If necessary, use --with-gtkgl-prefix=PREFIX

 guile

   use --with-guile

   If you use --with-guile you will need guile-config in your path

 python

   use --with-python
```

 Note that without either guile or python, coot is pretty crippled -
 and current the guile support is more developed - so use that for
 now, if you can.

 Building Coot
 ------------
To build coot, you will almost certainly need to specify the prefix
for clipper and mmdb.  Here is how I do it:

 ./configure --with-clipper-prefix=$HOME/crystal/clipper --with-mmdb-prefix=$HOME/crystal/mmdb112

Similarly you may also need to specify the path to gtkglarea libs using the
--with-gtkgl-prefix.

The following comments are copied (from the clipper and mmdb macros)
for you ease of reading:

 Example:

 So, here, for example, is the configure line used at the Daresbury
 development computer, dlpx1:

./configure --prefix=$HOME/gnu --with-gtk-prefix=/usr/local --with-gtkgl-prefix=$HOME/gnu --with-clipper-prefix=$HOME/cowtan --with-mmdb-prefix=$HOME/gnu --with-glib-prefix=/usr/local --with-glut-prefix=/usr/local

Here is how I configure using MacOS 10.2 (Darwin/fink):

./configure --prefix=$HOME/crystal --with-mmdb-prefix=$HOME/crystal --with-clipper-prefix=$HOME/crystal --with-glut-prefix=/sw --with-gl-prefix=/usr/X11R6 --with-guile

Redhat 7.2
 ./configure --prefix=$HOME/coot 
    --with-fftw-prefix=$HOME/coot
    --with-mmdb-prefix=$HOME/coot
    --with-gtkgl-prefix=$HOME/coot
    --with-clipper-prefix=$HOME/coot
    --with-gtkcanvas-prefix=$HOME/coot
    --with-guile

 SGI
 ---

  To use either the binary distribution or to use GTK_CANVAS (for
  Ramachandran plots), you will need to install the subsystem
  fw_imlib.sw.lib (imlib-1.9.9 from
  http://freeware.sgi.com/fw-6.2/index-by-alpha.html).  

  You will also need gcc and libungif.

  Note: Coot 0.1 moved to new style clipper and ccp4 libs and as a
  result I can no longer get clipper to compile on SGI with gcc/g++.
  Which means that Coot 0.1 won't work on SGI.  You can try to fix it
  - good luck.


Mac OS X
--------

The X11-dependent binary distribution for Mac OS X depends on having
some libraries installed by fink:

fink install gtk+ glib glib-shlibs guile16 guile16-libs guile16-shlibs guile16-dev glut glut-shlibs gtkglarea 
fink install imlib imlib-shlib

You also need Apple`s X11 SDK.

You need to work around a fink guile wrinkle.  The configure script
needs to know that guile-1.6 is actually guile and guile-1.6-config is
actually guile-config.  To do this I created links to those from a
directory in my path.  To be explicit:

$ mkdir ~/build/bin
$ ln -s /sw/bin/guile-1.6 ~/build/bin/guile
$ ln -s /sw/bin/guile-1.6-config ~/build/bin/guile-config


If you want to be able to refine and regularize (and I suggest that
you do) you will need to install the GNU Scientific Library (GSL).  I
suggest that you install it in $HOME/coot/Darwin.  You will need to
add $HOME/coot/Darwin/bin to your path before you configure coot.
Also, it`s a good idea to have gtk-canvas (installed in
$HOME/coot/Darwin) too.  Here`s how I configure gtk-canvas:

$ ./configure --prefix=$HOME/coot/Darwin --with-imlib-prefix=/sw

To compile, you will need to remove line 15 (the malloc line) from
gtk-canvas/gtk-canvas-load.c

If you use the binary tar, you will need to set the environment
variable DYLD_LIBRARY_PATH.  Say you untared the binary distribution
into $HOME/coot, then you would need to add setup (tcsh) setup code of
the following form:
```
if ($?DYLD_LIBRARY_PATH) then
   setenv DYLD_LIBRARY_PATH $(DYLD_LIBRARY_DIR}:$HOME/coot/lib
else
   setenv DYLD_LIBRARY_PATH $HOME/coot/lib
endif

See also http://www.ysbl.york.ac.uk/~emsley/coot/setup

The default mouse focusing with the Aqua window manager is a PITA when 
using coot. Try this:

$ defaults write com.apple.x11 wm_ffm true
```

Cygwin
------

To extend the available memory:
http://cygwin.com/cygwin-ug-net/setup-maxmem.html


Ligand Dialog, aka LIDIA
------------------------

To get ligand builder dialog you will need to compile with a
sufficiently recent version of gtk (2.16 or later) and goocavanvas.


extension language wrapper
--------------------------

If you get:

../src/c-inner-main.c:76: undefined reference to `SWIG_init`

Solution coot_wrap_guile.cc needs to be updated now that guile is 
	 being used:

	 $ rm coot_wrap_guile.cc
	 $ make coot_wrap_guile.cc
	 $ make

Note that if you use the --with-python argument to coot`s configure,
then you will need by hand to install (copy) the resulting src/coot.py
into $COOT_PREFIX/share/coot/python directory.



Compiling For Multiple Architectures
====================================

   You can compile the package for more than one kind of computer at the
same time, by placing the object files for each architecture in their
own directory.  To do this, you must use a version of `make` that
supports the `VPATH` variable, such as GNU `make`.  `cd` to the
directory where you want the object files and executables to go and run
the `configure` script.  `configure` automatically checks for the
source code in the directory that `configure` is in and in `..`.

   If you have to use a `make` that does not supports the `VPATH`
variable, you have to compile the package for one architecture at a time
in the source code directory.  After you have installed the package for
one architecture, use `make distclean` before reconfiguring for another
architecture.

Installation Names
==================

   By default, `make install` will install the package's files in
`/usr/local/bin`, `/usr/local/man`, etc.  You can specify an
installation prefix other than `/usr/local` by giving `configure` the
option `--prefix=PATH`.

   You can specify separate installation prefixes for
architecture-specific files and architecture-independent files.  If you
give `configure` the option `--exec-prefix=PATH`, the package will use
PATH as the prefix for installing programs and libraries.
Documentation and other data files will still use the regular prefix.

   In addition, if you use an unusual directory layout you can give
options like `--bindir=PATH` to specify different values for particular
kinds of files.  Run `configure --help` for a list of the directories
you can set and what kinds of files go in them.

   If the package supports it, you can cause programs to be installed
with an extra prefix or suffix on their names by giving `configure` the
option `--program-prefix=PREFIX` or `--program-suffix=SUFFIX`.

Optional Features
=================

   Some packages pay attention to `--enable-FEATURE` options to
`configure`, where FEATURE indicates an optional part of the package.
They may also pay attention to `--with-PACKAGE` options, where PACKAGE
is something like `gnu-as` or `x` (for the X Window System).  The
`README` should mention any `--enable-` and `--with-` options that the
package recognizes.

   For packages that use the X Window System, `configure` can usually
find the X include and library files automatically, but if it doesn`t,
you can use the `configure` options `--x-includes=DIR` and
`--x-libraries=DIR` to specify their locations.

Specifying the System Type
==========================

   There may be some features `configure` can not figure out
automatically, but needs to determine by the type of host the package
will run on.  Usually `configure` can figure that out, but if it prints
a message saying it can not guess the host type, give it the
`--host=TYPE` option.  TYPE can either be a short name for the system
type, such as `sun4`, or a canonical name with three fields:
     CPU-COMPANY-SYSTEM

See the file `config.sub` for the possible values of each field.  If
`config.sub` isn`t included in this package, then this package doesn`t
need to know the host type.

   If you are building compiler tools for cross-compiling, you can also
use the `--target=TYPE` option to select the type of system they will
produce code for and the `--build=TYPE` option to select the type of
system on which you are compiling the package.

Sharing Defaults
================

   If you want to set default values for `configure` scripts to share,
you can create a site shell script called `config.site` that gives
default values for variables like `CC`, `cache_file`, and `prefix`.
`configure` looks for `PREFIX/share/config.site` if it exists, then
`PREFIX/etc/config.site` if it exists.  Or, you can set the
`CONFIG_SITE` environment variable to the location of the site script.
A warning: not all `configure` scripts look for a site script.

Operation Controls
==================

   `configure` recognizes the following options to control how it
operates.

`--cache-file=FILE`
     Use and save the results of the tests in FILE instead of
     `./config.cache`.  Set FILE to `/dev/null` to disable caching, for
     debugging `configure`.

`--help`
     Print a summary of the options to `configure`, and exit.

`--quiet`
`--silent`
`-q`
     Do not print messages saying which checks are being made.  To
     suppress all normal output, redirect it to `/dev/null` (any error
     messages will still be shown).

`--srcdir=DIR`
     Look for the package's source code in directory DIR.  Usually
     `configure` can determine that directory automatically.

`--version`
     Print the version of Autoconf used to generate the `configure`
     script, and exit.

--with-mmdb-prefix=PREFIX
     Use PREFIX as the directory in which mmdb has been built or installed. 

     If you are using the non-installed version, in the mmdb-prefix you
     will see PDBCur, Cont, makemmdb etc.  Link (or copy) mmdb.a to
     libmmdb.a (and that it is in the top directory of mmdb).

     If you are using the installed version, in the mmdb-prefix you
     will see lib, include (etc).

--with-clipper-prefix=PREFIX
     Use PREFIX as the directory in which clipper has been built.

     The clipper PREFIX you will normally find the downloaded .tar.gz
     files (fftw-2.1.3.tar.gz umtz.latest.tar.gz clipper.latest.tar.gz
     etc).



Usage:
-----

Environment Variables: 

  COOT_REF_STRUCTS:

    By default Coot looks in
    $prefix/share/coot/reference-structures, if not there then it
    checks the environment variable COOT_REF_STRUCTS (see below).

  COOT_STANDARD_RESIDUES:
   
    By defaults Coot looks in
    $prefix/share/coot/standard-residues.pdb, if not there then it
    checks the environment variable COOT_STANDARD_RESIDUES.

  COOT_SCHEME_DIR

    By defaults Coot looks in $prefix/share/coot/scheme for its scheme
    extension files, if not there then it checks in the directory
    pointed to by the environment variable COOT_SCHEME_DIR.
  
Reference Structures

  COOT_REF_STRUCTS is an environment variable that points to a
  directory of reference structures [these structures are used to
  generate a fragment library for the generation of the mainchain from
  Ca coordinates].  If you aren`t building mainchain, you don't need
  them.  A set of high resolution (1.3A or better) is available from: 

  http://www.ysbl.york.ac.uk/~emsley/software/coot-reference-structures.tar.gz


"External" Scheme Scripts
-------------------------

Ths error:

Loading scheme files from /usr/local/coot/share/coot/scheme
load "filter.scm"
load "coot-utils.scm"
load "coot-gui.scm"
(Error in proc: misc-error  args:  (#f ~A ~S (no code for module (gui
event-loop)) #f))
(Error in proc: unbound-variable  args:  (#f Unbound variable: ~S
(tips-gui) #f))

is fixed by installing the guile-gui package.

This error:

(...)
Loading scheme files from /usr/local/coot/share/coot/scheme
load "filter.scm"
load "coot-utils.scm"
load "coot-gui.scm"
Coot Scripting GUI code found and loaded.
load "coot-lsq.scm"
load "shelx.scm"
load "get-ebi.scm"
(Error in proc: misc-error  args:  (#f ~A ~S (no code for module (net
http)) #f))
(Error in proc: unbound-variable  args:  (#f Unbound variable: ~S
(tips-gui) #f))

is fixed by installing the net-http package.


