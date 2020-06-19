# Radare 2
Kali comes pre-installed with radare2. You can check by doing:
```
$ radare2 --version
```
If it's not installed, then we can either download it from apt
```
$ apt-get install radare2
```
## Windows:
Or you can install it from github:
```
$ git clone https://github.com/radare/radare2.git
$ cd radare2
$ sys/install.sh
```
**(Or without root permissions)**
```
$ sys/user.sh
```
And then to run it:
```
$ r2
```
# Ghidra:
Ghidra is a tool that the NSA released for it to be open source back in April 2019, and it's an amazing tool.\
* Official download link: https://www.ghidra-sre.org/ 
* Github page: https://github.com/NationalSecurityAgency/ghidra
* Full installation guide found here: https://ghidra-sre.org/InstallationGuide.html

The software needed to install ghidra is as follows (taken exactly from the installation guide):\
* Java 11 64-bit Runtime and Development Kit (JDK) 
* Free long term support (LTS) versions of JDK 11 are provided here: 
  * AdoptOpenJDK
  * Amazon Corretto

If, for whatever reason, your installation doesn't go as expected, here is ghidra's troubleshooting page: https://ghidra-sre.org/InstallationGuide.html#Troubleshooting

## Linux:
Once you've downloaded ghidra as a zip file, you need to unzip:
```
$ unzip ghidra_*_PUBLIC_*.zip
$ sudo apt-get install default-jdk
```
Change directories into the ghidra folder, and then run using 
```
$ ./ghidraRun
```
## Windows:

Extract the zip file to any location that you would like (I'm going to be using the desktop)
  * Right click on zip -> extract all / extract (here)

Open the Environment Tables window:
  * Right click on windows start button -> Click on "System"
  * Click "Advanced System Settings" on the left (Administrative priveleges needed)
  * Click "Environment Variables" at the bottom

Add the JDK bin directory to the PATH variable
  * Under "System Variables", click "Path" -> edit -> new
  * Type the path of where you extracted the zip to + "\bin"
  * Click "ok" -> "ok" -> "ok"

To run:
  * Navigate to zip extraction path
  * Run ghidra.bat
