RPMs for gfc-gfortran
=========================
The files in this repository are used to build RPMs for `gfc-gfortran`.

About this repository
=====================
For instructions on building RPMs for `gfc-gfortran` see below.
For additional help, you may find it helpful to consult the [Fedora RPM Guide](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch-creating-rpms.html).

Suitable RPMs for RHEL6-based systems are currently in development. When they
are functional, the RPMs themselves will be released and can be used to easily
install `gfc-gfortran`.

Dependencies
============
To build RPMs for `gfc-gfortran`, you must have at least the following
packages installed. For multilib support, the 32-bit compatibility libraries
for glibc-devel must also be installed.

```
rpm-build
sharutils
glibc-devel
```

Building the RPMs
=================
To begin, create an `rpmbuild` directory somewhere. Place the following in
`~/.rpmmacros` (replacing `$RPMBUILD` with your `rpmbuild` directory):

```
%_topdir $RPMBUILD
```

Checkout this repository into `rpmbuild` and grab the required source files:

```bash
mkdir $RPMBUILD
hg clone http://user@repo.rincon.com:5000/M50/gfc-rpm $RPMBUILD
cd $RPMBUILD/SOURCES
wget ftp://gcc.gnu.org/pub/gcc/infrastructure/gmp-4.3.2.tar.bz2
wget ftp://gcc.gnu.org/pub/gcc/infrastructure/mpfr-2.4.2.tar.bz2
wget ftp://gcc.gnu.org/pub/gcc/infrastructure/mpc-0.8.1.tar.gz
wget ftp://gcc.gnu.org/pub/gcc/releases/gcc-4.8.3/gcc-4.8.3.tar.bz2
```

(Note: The gcc sources are large (~80MB); if you find the download too slow,
you should try one of the [GNU mirror sites](http://www.gnu.org/prep/ftp.html).
Just replace [ftp://gcc.gnu.org/pub] with the URL of a site near you.)

Finally build the binary RPMs:

```bash
cd $RPMBUILD
rpmbuild -bb SPECS/gfc-gcc.spec
```

After a few hours you will find some RPMs in the `RPMS/$(uname -i)`
subdirectory.  You only need `gfc-gfortran`, `gfc-gcc`, `gfc-c++`, and their
dependencies `gfc-cpp`, `libgfortran`, `libgcc`, `libgomp`, `libquadmath`,
`libquadmath-devel`, `libstdc++`, and `libstdc++-devel`.

Installing the RPMs
===================
Install pieces of this package with the following commands:

```bash
# Install gfc-gcc
rpm -i libgcc*.rpm \
    -i libgomp*.rpm \
    -i gfc-cpp*.rpm \
    -i gfc-gcc*.rpm

# Install gfc-gfortran
rpm -i libquadmath*.rpm \
    -i libquadmath-devel*.rpm \
    -i libgfortran*.rpm \
    -i gfc-gfortran*.rpm 

# Install gfc-g++
rpm -i libstdc++-devel*.rpm \
    -i libstdc++*.rpm \
    -i gfc-c++*.rpm 
```

Usage
=====
To allow installation side-by-side with your system gcc, these RPMs place the
libraries in a special place (`/usr/lib/gcc/4.8.3-for/<arch>/`). The compiler
collection is built to look here for supporting libraries, but executables
built by these compilers will not know to look there for runtime support.
Therefore you have two options:

 * Add `-Wl,-rpath,/usr/lib/gcc/4.8.3-for/<arch>/` to LFLAGS when linking
   (done automatically in Midas 5.0 when configured for the GFC compilers)

 * Set your `LD_LIBRARY_PATH` to point to `/usr/lib/gcc/4.8.3-for/<arch>/`
   before /lib[64]/ and /usr/lib[64]/

The first option is preferred, as it tells the executable where to look without
bashing your system configuration.

The second option will override the default system libraries, which you should
only do if you know what you're doing.

Note that in either case you will need to replace `<arch>` with the actual
directory present in that location for your machine (for example, x86_64 RHEL
machines tend to use something like `x86_64-redhat-linux`).
