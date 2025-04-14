## sphinxbase

The last (good) version written in C. Build, test, and install this before doing the same with pocketsphinx.

Get the 4.0.0 code [here](https://sourceforge.net/projects/cmusphinx/files/).

Create folder structure and install sphinxbase prerequisites
		
```
     su
     mkdir projects
     mkdir software
     apt install gcc automake autoconf libtool bison swig audacity libasound2-dev python-dev-is-python3 python3-pip
```

Clone and install sphinxbase (NOT as sudo)
	Navigate to software storage directory
		`cd ~/software`
	Clone the sphinxbase repo
		`git clone https://github.com/cartheur/sphinxbase.git`
	Step into the new directory
 
```
		cd sphinxbase
		./autogen.sh
		make
		sudo make install
```
If the autogen.sh file returns a permission denied error:

```
chmod +x autogen.sh
```

Check the installation
		`sphinx_lm_convert`
An error will be experienced. Add path to the location where sphinx is installed
		`sudo nano /etc/ld.so.conf`
Add new line
		`/usr/local/lib`
Refresh the configuration
		`sudo ldconfig`
Retest the installation
		`sphinx_lm_convert`
