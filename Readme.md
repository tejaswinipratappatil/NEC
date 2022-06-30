# Voice2json
<br>

## Getting Started
<br>

### Installing [voice2json](http://voice2json.org)
Voice2json has been tested on Python 3.7 and [Ubuntu 18.04](https://releases.ubuntu.com/18.04/).

#### Pre-requisite
* [Ubuntu 18.04](https://releases.ubuntu.com/18.04/)
* [Python 3.7](https://www.python.org/downloads/release/python-370/)


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
  
If voice2json in ```PATH``` is not executable then run the following command to make it executable :
  
  ```chmod +x /path/to/voice2json```

If you are building from source on macOS, you will also need to install coreutils:
```
$ brew install coreutils
```

  <br>

### Package Installations and Enviornment Setup
  
```
apt update
apt install sudo
sudo apt install software-properties-common
sudo apt install python3.7
sudo apt install gcc
apt install curl
sudo apt-get install libatlas-base-dev liblapack-dev libblas-dev
apt install make
sudo apt install swig
apt-get install python3-venv
sudo rm /usr/bin/python3 #remove existing link
sudo ln /usr/bin/python3.7 /usr/bin/python3
```
```
apt install git
sudo apt-get install python3.7-dev
apt-get install vim
```
  
<br>
  
### Training the model

* We add our sentences in sentences.ini file
* Train the profile using "voice2json train-profile" command.
* We can check the path for sentence.ini file using "voice2json print-files" command
* After training if some words from sentence.ini files are not there then we neeed to add them in custom_words.txt file  
  
  <br>
  
### Post Installation
  
#### Download Profile
  
```voice2json``` must have a profile in order to do speech/intent recognition. Because the artifacts for each language/locale can be quite large (100’s of MB or more), ```voice2json``` does not include them in its [Debian package](http://voice2json.org/install.html#debian-package), [Docker image](http://voice2json.org/install.html#docker-image), or [source repository](http://voice2json.org/install.html#from-source).

To download artifacts for a specific profile or language, use:
```
$ voice2json --debug --profile <PROFILE> download-profile
```
where ```<PROFILE>``` is one of the [supported languages](http://voice2json.org/#supported-languages) (like en or fr), or one of the [known profile](https://github.com/synesthesiam/voice2json/tree/master/etc/profiles) names like ```de_kaldi-zamia```.
  
####  Example for download a profile "en-us_kaldi-zamia"
```
  $ voice2json --profile  en-us_kaldi-zamia download-profile
```

Note: The post-download process may take a long time, especially on Raspberry Pi (SD card). This is because ```voice2json``` is decompressing and re-combining split files from [GitHub](https://github.com/synesthesiam/voice2json-profiles).

Once everything is downloaded (by default in ```$HOME/.local/share/voice2json```), you should be able to train your profile:
```
$ voice2json --debug --profile <PROFILE> train-profile
```
If you manually download or move files to the ```$HOME/.config/voice2json directory```, you may omit the ```--profile``` argument to ```voice2json```.

#### Test Your Profile
  
Once you’ve trained your profile, you can quickly test it out with:
```
$ voice2json --profile <PROFILE> transcribe-stream
```  
The ```transcribe-stream``` will record from your microphone (using ```arecord``` and the default device), wait for you to speak a voice command, and then output a transcription (hit CTRL + C to exit).

If you’re using the default English sentences, try saying “turn on the living room lamp” and wait for the output. Getting intents out is as easy as:
```
$ voice2json --profile <PROFILE> transcribe-stream | \
    voice2json --profile <PROFILE> recognize-intent 
```  
Speaking a voice command should now output a line of JSON with the recognized intent. For example, “what time is it” outputs something like:
```
{
  "text": "what time is it",
  "likelihood": 0.025608657540496446,
  "transcribe_seconds": 1.4270143630001257,
  "wav_seconds": 0.0043125,
  "tokens": [
    "what",
    "time",
    "is",
    "it"
  ],
  "timeout": false,
  "intent": {
    "name": "GetTime",
    "confidence": 1
  },
  "entities": [],
  "raw_text": "what time is it",
  "recognize_seconds": 0.00019677899945236277,
  "raw_tokens": [
    "what",
    "time",
    "is",
    "it"
  ],
  "speech_confidence": null,
  "wav_name": null,
  "slots": {}
}
```
  
<br>
  
#### Back Up Your Profile
  
If you have an existing ```voice2json``` profile, it is highly recommended you regularly back up the following files:

* ```sentences.ini``` - your custom voice commands
* ```custom_words.txt``` - your custom pronunciations
* ```profile.yml``` - your custom settings
* ```slots``` - directory with custom slot values
* ```slot_programs``` - directory with custom slot programs
* ```converters``` - directory with custom conversion programs
  
See the ```print-files``` for an easy way to automate backups.  


#### To Run Python Script:
After sucessfully installation of voice2json need to run python script.
To run python script need to run following command
  
``` 
  $ python3 transcribestream.py
```

It will print "Press space to start".  After pressing the space bar, It will start the listing command. 
Speak the command that you need to convert to json. After that It will open file to get the json and close it after some time. 
After closing the file it will again the print  "Press space to start". So it will continous listen the voice commands.
