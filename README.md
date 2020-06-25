RPMs for gfc-gfortran
=========================
The files in this repository are used to build RPMs for `gfc-gfortran`.

About this repository
=====================
For instructions on building RPMs for `gfc-gfortran` see below.
For additional help, you may find it helpful to consult the [Fedora RPM Guide](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch-creating-rpms.html).

See the [Releases](https://github.com//fritzr/gfc-rpmbuild/releases) page for pre-built binary RPMs built for CentOS 6 and 7 on x86_64.

Dependencies
============
To build RPMs for `gfc-gfortran`, you must have at least the following
packages installed. For multilib support, the 32-bit compatibility libraries
for glibc-devel must also be installed.

```
rpm-build
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
wget http://vault.centos.org/7.2.1511/os/Source/SPackages/gcc-4.8.5-4.el7.src.rpm
rpm2cpio gcc-4.8.5-4.el7.src.rpm | cpio -di
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
subdirectory. 

For pre-built releases of the binary RPMs produced by this process,
see the [Releases](https://github.com/fritzr/gfc-rpmbuild/releases) page.

Installing the RPMS
===================

The binary RPMs are built for CentOS 6 or 7 on x86_64. On these platforms,
installing the RPMs is simple:

```bash
yum localinstall gfc-*.rpm
```
