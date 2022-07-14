---
layout: default
title: GNURadio UHD Linux Toolchain
parent: USRP
nav_order: 1
---

# [Building and Installing the USRP Open-Source Toolchain (UHD and GNU Radio) on Linux](https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_Linux)
### Update and Install dependencies
Before building UHD and GNU Radio, you need to make sure that all the dependencies are first installed.
However, before installing any dependencies, you should first make sure that all the packages that are already installed on your system are up-to-date. You can do this from a GUI, or from the command-line, as shown below.
On Ubuntu systems, run:
```sh
sudo apt-get update
```
On Ubuntu 18.04 systems, run:
```sh
sudo apt-get -y install git swig cmake doxygen build-essential libboost-all-dev libtool libusb-1.0-0 libusb-1.0-0-dev libudev-dev libncurses5-dev libfftw3-bin libfftw3-dev libfftw3-doc libcppunit-1.14-0 libcppunit-dev libcppunit-doc ncurses-bin cpufrequtils python-numpy python-numpy-doc python-numpy-dbg python-scipy python-docutils qt4-bin-dbg qt4-default qt4-doc libqt4-dev libqt4-dev-bin python-qt4 python-qt4-dbg python-qt4-dev python-qt4-doc python-qt4-doc libqwt6abi1 libfftw3-bin libfftw3-dev libfftw3-doc ncurses-bin libncurses5 libncurses5-dev libncurses5-dbg libfontconfig1-dev libxrender-dev libpulse-dev swig g++ automake autoconf libtool python-dev libfftw3-dev libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77 libsdl1.2-dev python-wxgtk3.0 git libqt4-dev python-numpy ccache python-opengl libgsl-dev python-cheetah python-mako python-lxml doxygen qt4-default qt4-dev-tools libusb-1.0-0-dev libqwtplot3d-qt5-dev pyqt4-dev-tools python-qwt5-qt4 cmake git wget libxi-dev gtk2-engines-pixbuf r-base-dev python-tk liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2 libzmq3-dev libzmq5 python-requests python-sphinx libcomedi-dev python-zmq libqwt-dev libqwt6abi1 python-six libgps-dev libgps23 gpsd gpsd-clients python-gps python-setuptools
```
On Ubuntu 16.04 systems, run:
```sh
sudo apt-get -y install git swig cmake doxygen build-essential libboost-all-dev libtool libusb-1.0-0 libusb-1.0-0-dev libudev-dev libncurses5-dev libfftw3-bin libfftw3-dev libfftw3-doc libcppunit-1.13-0v5 libcppunit-dev libcppunit-doc ncurses-bin cpufrequtils python-numpy python-numpy-doc python-numpy-dbg python-scipy python-docutils qt4-bin-dbg qt4-default qt4-doc libqt4-dev libqt4-dev-bin python-qt4 python-qt4-dbg python-qt4-dev python-qt4-doc python-qt4-doc libqwt6abi1 libfftw3-bin libfftw3-dev libfftw3-doc ncurses-bin libncurses5 libncurses5-dev libncurses5-dbg libfontconfig1-dev libxrender-dev libpulse-dev swig g++ automake autoconf libtool python-dev libfftw3-dev libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77 libsdl1.2-dev python-wxgtk3.0 git-core libqt4-dev python-numpy ccache python-opengl libgsl-dev python-cheetah python-mako python-lxml doxygen qt4-default qt4-dev-tools libusb-1.0-0-dev libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4 cmake git-core wget libxi-dev gtk2-engines-pixbuf r-base-dev python-tk liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2 libzmq-dev libzmq1 python-requests python-sphinx libcomedi-dev python-zmq python-setuptools
```

### Building and installing UHD from source code
First, make a folder to hold the repository.
```sh
cd $HOME
mkdir workarea
cd workarea
```
Next, clone the repository and change into the cloned directory.
```sh
git clone https://github.com/EttusResearch/uhd
cd uhd
```
Next, checkout the desired UHD version. You can get a full listing of tagged releases by running the command:
```sh
git tag -l
```
Example truncated output of `git tag -l`:
```sh
$ git tag -l
...
release_003_009_004
release_003_009_005
release_003_010_000_000
```
After identifying the version and corresponding release tag you need, check it out:
```sh
git checkout v3.14.0.0
```
Next, create a build folder within the repository, invoke CMake and build UHD.
```sh
cd host
mkdir build
cd build
cmake ../
make -j 12
```
Next, you can optionally run some basic tests to verify that the build process completed properly.
```sh
make test -j 12
```
Next, install UHD, using the default install prefix, which will install UHD under the /usr/local/lib folder. You need to run this as root due to the permissions on that folder.
```sh
sudo make install
```
Next, update the system's shared library cache.
```sh
sudo ldconfig
```
Finally, make sure that the `LD_LIBRARY_PATH` environment variable is defined and includes the folder under which UHD was installed. Most commonly, you can add the line below to the end of your `$HOME/.bashrc` file:
```sh
export LD_LIBRARY_PATH=/usr/local/lib
```
### Downloading the UHD FPGA Images
You can now download the UHD FPGA Images for this installation. This can be done by running the command `uhd_images_downloader`.
```sh
$ sudo uhd_images_downloader
```
### Updating the FPGA
Power on the USRP and run
```sh
uhd_image_loader --args="type=x300,addr=192.168.10.2,fpga=HG"
```
### Building and installing GNU Radio from source code
First, make a folder to hold the repository.
```sh
cd $HOME
cd workarea
```
Next, clone the repository.
```sh
git clone --recursive https://github.com/gnuradio/gnuradio
```
Next, go into the repository and check out the desired GNU Radio version.
```sh
cd gnuradio
```
To checkout the `v3.7.13.4` branch:
```sh
git checkout v3.7.13.4
```
Next, update the submodules:
```sh
git submodule update --init --recursive
```
Next, create a build folder within the repository, invoke CMake, and build GNU Radio:
```sh
mkdir build
cd build
cmake ../
make -j 12
```
Next, you can optionally run some basic tests to verify that the build process completed properly.
```sh
make test -j 12
```
Next, install GNU Radio, using the default install prefix, which will install GNU Radio under the /usr/local/lib folder. You need to run this as root due to the permissions on that folder.
```sh
sudo make install
```
Finally, update the system's shared library cache.
```sh
sudo ldconfig
```
At this point, GNU Radio should be installed and ready to use. You can quickly test this, with no USRP device attached, by running the following quick tests.
```sh
gnuradio-config-info --version
gnuradio-config-info --prefix
gnuradio-config-info --enabled-components
```
There is a simple flowgraph that you can run that does not require any USRP hardware. It's called the dialtone test, and it produces a PSTN dial tone on the computer's speakers. Running it verifies that all the libraries can be found, and that the GNU Radio run-time is working.
```sh
python $HOME/workarea/gnuradio/gr-audio/examples/python/dial_tone.py
```
You can try launching the GNU Radio Companion (GRC) tool, a visual tool for building and running GNU Radio flowgraphs.
```sh
gnuradio-companion
```
If "gnuradio-companion" does not start and complains about the PYTHONPATH environment variable, then you may have to set this in your $HOME/.bashrc file, as shown below.
```sh
export PYTHONPATH=/usr/local/lib/python2.7/dist-packages
```
