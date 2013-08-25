boxeebox-xbmc
=============

Aiming to get xbmc up and running on the boxee box.

The build scripts will download, compile and install all dependencies,
including cross compilers. In other words, you don't need to have any
Boxeebox or XBMC related code or SDK's installed on your system and even if you had
they won't be used.

@quarnster is performing his builds on OSX 10.8.3, but occasionally tests with a Ubuntu VM.
Pull requests (if needed) enabling this to work on other systems are welcome.

# Prerequisites

Ubuntu seemed to require:
```
sudo apt-get install cmake make gcc texinfo gawk bison flex swig default-jre gperf
```

I'm pretty sure my OSX brew line was similar.

Your system must be capable of executing "configure" scripts and your filesystem needs
to be case sensitive.

# Building

```bash
git clone https://github.com/quarnster/boxeebox-xbmc.git
cd boxeebox-xbmc
mdkir build
cd build
cmake -DTARGET_DIR=<WHERE_YOU_WANT_TO_INSTALL_XBMC> ..
make
```

Please do submit a pull request with any tweaks needed to make it build alright this far.

## Making changes to libs/*, toolchain/* or xbmc/*

Changes to these files are unfortunately not picked up automatically, so you'll have to
manually copy any modified files and make the build system respect your changes via
for example:

```
cp ../toolchain/* toolchain-prefix/src/toolchain/ && touch toolchain-prefix/src/toolchain-stamp/toolchain-download
cp ../libs/* libs-prefix/src/libs/ && touch libs-prefix/src/libs-stamp/libs-download
cp ../xbmc/* xbmc-prefix/src/xbmc/ && touch xbmc-prefix/src/xbmc-stamp/xbmc-download
```

## Installing

Once everything has been built, the final target distribution can be created with:

```bash
make -j8 shares # only needed once unless you tweak any of the share files
make dist
```

## Running

Telnet into your boxeebox and run:
```
/etc/rc3.d/U99boxee stop
killall run_boxee.sh
killall Boxee
/etc/rc3.d/U94boxeehal stop
killall BoxeeHal
cd <WHATEVER_YOUR_TARGET_DIR_WAS>
./xbmc.bin --debug -fs --standalone
```

# References
### Cross compilation reference material

* LFS: http://www.linuxfromscratch.org/lfs/view/stable/
* Python cross compilation: http://randomsplat.com/id5-cross-compiling-python-for-embedded-linux.html
* + my own 12 years or so of cross compilation experience...

### Boxeebox reference material

* Intel SDK originally downloaded from http://bbx.boxee.tv/download/?ver=source&early=0. Bits of it is available as a newer version at https://code.google.com/p/googletv-mirrored-source/source/checkout?repo=intel-sdk.
* Boxeebox source code https://code.google.com/p/bawx/
* http://www.boxeeboxwiki.org
* http://gtvhacker.com/index.php/Boxee
