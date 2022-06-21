# Voice2json
<br>

## Getting Started
<br>

### Installing [voice2json](http://voice2json.org)
Voice2json has been tested on [Ubuntu 18.04](https://releases.ubuntu.com/18.04/), the newer versions of Ubuntu does not support the packages involved.

There are three different options to install voice2json:
* [Debian Package](http://voice2json.org/install.html#debian-package)
* [Docker Image](http://voice2json.org/install.html#docker-image)
* [From Source](http://voice2json.org/install.html#docker-image)

<br>

### Debian Package
Pre-compiled packages are available for Debian-based distributions (Ubuntu, Linux Mint, etc.) on ```amd64```, ```armhf```, and ```arm64 (aarch64)``` architectures. These packages are built using Docker and ```dpkg```.

Next, download the appropriate .deb file for your CPU architecture:

[amd64](https://github.com/synesthesiam/voice2json/releases/download/v2.1/voice2json_2.1_amd64.deb) - Desktops, laptops, and servers
[armhf](https://github.com/synesthesiam/voice2json/releases/download/v2.1/voice2json_2.1_armhf.deb) - Raspberry Pi 2, and 3/3+ (armv7)
[arm64](https://github.com/synesthesiam/voice2json/releases/download/v2.1/voice2json_2.1_arm64.deb) - Raspberry Pi 3+, 4

If you’re unsure about your architecture, run:
```
$ dpkg-architecture | grep DEB_BUILD_ARCH=
```
which will output something like:
```
DEB_BUILD_ARCH=amd64
```
Next, install the .deb file:
```
$ sudo apt install /path/to/voice2json_<VERSION>_<ARCH>.deb
```
where where <VERSION> is voice2json’s version (probably 2.1) and <ARCH> is your build architecture.

NOTE: If you run sudo apt install in the same directory as the .deb file, make sure to prefix the filename with ./ like this:
  
```
$ sudo apt install ./voice2json_<VERSION>_<ARCH>.deb
```
After [downloading a profile](http://voice2json.org/commands.html#download-profile), you should now be able to run any of the example voice2json commands in the documentation.
