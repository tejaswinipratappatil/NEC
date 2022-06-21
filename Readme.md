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

NOTE: If you run sudo apt install in the same directory as the .deb file, make sure to prefix the filename with ./ like this
  
```
$ sudo apt install ./voice2json_<VERSION>_<ARCH>.deb
```
After [downloading a profile](http://voice2json.org/commands.html#download-profile), you should now be able to run any of the example voice2json commands in the documentation.
  
<br>

### Docker Image
  
The easiest way to try out ```voice2json``` is with Docker. Pre-built images are available for ```amd64```, ```armhf```, ```armel```, and ```arm64 (aarch64)``` CPU architectures. To get started, make sure you have Docker installed:
```
$ curl -sSL https://get.docker.com | sh
```
and that your user is part of the docker group:
```
$ sudo usermod -a -G docker $USER
```
**Be sure to reboot** after adding yourself to the docker group!

#### Shell Script
  
Create a Bash script named voice2json somewhere in your $PATH and add the following content ([source](https://github.com/synesthesiam/voice2json/blob/master/docker/voice2json)):
```
#!/usr/bin/env bash
docker run -i \
       --init \
       -v "${HOME}:${HOME}" \
       -v "/dev/shm/:/dev/shm/" \
       -w "$(pwd)" \
       -e "HOME=${HOME}" \
       --user "$(id -u):$(id -g)" \
       synesthesiam/voice2json "$@"`
```
Mark it as executable with ```chmod +x /path/to/voice2json``` and try it out:
```
$ voice2json --help
```
After [downloading a profile](http://voice2json.org/commands.html#download-profile), you should now be able to run any of the example voice2json commands in the documentation.

#### Microphone Access
  
Getting Docker to properly use your microphone can be difficult. For commands like ```transcribe-stream``` that operate on a live audio stream, try:

Adding ```--device /dev/snd:/dev/snd``` to your docker run command, or
Piping audio in with something like ```arecord -r 16000 -f S16_LE -c 1 | voice2json transcribe-stream --audio-source -```
Updating
To update your voice2json Docker image, simply run:
```
$ docker pull synesthesiam/voice2json
```
<br>

### From Source

```voice2json``` uses autoconf to facilitate building from source. You will need ```Python 3.7``` and some common build tools like gcc.
  
Packages required:
* GCC
* Port Audio
* Swig
* PocketSphinx  
 
Once you’ve cloned the the repository, the build steps should be familiar:
```
$ git clone https://github.com/synesthesiam/voice2json
$ cd voice2json
$ ./configure
$ make
$ make install
```
This will install ```voice2json``` inside a virtual environment at ```$PWD/.venv``` by default with all of the supported speech to text engines and supporting tools. When installation is finished, copy ```voice2json.sh``` somewhere in your ```PATH``` and rename it to ```voice2json```.

If you are building from source on macOS, you will also need to install coreutils:
```
$ brew install coreutils
```
