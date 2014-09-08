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

Finally build gfc-gfortran and required RPMs:

```bash
cd $RPMBUILD
rpmbuild -bb SPECS/gfc.spec
```

After an hour or two you will find some RPMs in the `RPMS/<arch>` subdirectory.
You only need `gfc-gfortran`, `gfc`, `gfc-c++`, and their dependencies
`cpp`, `libgfortran`, `libgcc`, `libgomp`, `libquadmath`, `libquadmath-devel`,
`libstdc++`, and `libstdc++-devel`.

Installing the RPMs
===================
Install the RPMs by running the following as root:

```bash
rpm -i cpp-4.8.3-4.el6.x86_64.rpm gfc-4.8.3-4.el6.x86_64.rpm gfc-c++-4.8.3-4.el6.x86_64.rpm gfc-gfortran-4.8.3-4.el6.x86_64.rpm libgcc-4.8.3-4.el6.x86_64.rpm libgfortran-4.8.3-4.el6.x86_64.rpm libgomp-4.8.3-4.el6.x86_64.rpm libquadmath-4.8.3-4.el6.x86_64.rpm libquadmath-devel-4.8.3-4.el6.x86_64.rpm libstdc++-4.8.3-4.el6.x86_64.rpm libstdc++-devel-4.8.3-4.el6.x86_64.rpm
```

You should now have `gfc-gfortran`, `gfc-gcc`, and `gfc-g++`!
