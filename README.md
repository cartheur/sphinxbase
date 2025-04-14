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

### Newer errors

With the depreciation of `distutils` in python3.12, even using 3.10.8 will result in errors:

`checking for the distutils Python package... no`

Even if using `sudo dpkg -l | grep python3` where you can clearly see:

```
python3-distutils               3.10.8-1~22.04                              all          distutils package for Python 3.x
python3-setuptools              59.6.0-1.2ubuntu0.22.04.2                   all          Python3 Distutils Enhancements

```
and `python3 -c "import distutils; print(distutils.__file__)"` shows:

```
<string>:1: DeprecationWarning: The distutils package is deprecated and slated for removal in Python 3.12. Use setuptools or check PEP 632 for potential alternatives
/usr/lib/python3.10/distutils/__init__.py
```

It still fails.

### Notes getting this solved

Left off with searching the directory for `grep -rnw ~/github/sphinxbase -e 'distutils'` and finding:

```
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:136:	# Check if you have distutils, else fail
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:138:	AC_MSG_CHECKING([for the distutils Python package])
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:139:	ac_distutils_result=`$PYTHON -c "import distutils" 2>&1`
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:144:		AC_MSG_ERROR([cannot import Python module "distutils".
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:155:		python_path=`$PYTHON -c "import distutils.sysconfig; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:156:			print (distutils.sysconfig.get_python_inc ());"`
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:157:		plat_python_path=`$PYTHON -c "import distutils.sysconfig; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:158:			print (distutils.sysconfig.get_python_inc (plat_specific=1));"`
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:182:from distutils.sysconfig import *
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:205:import distutils.sysconfig
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:206:e = distutils.sysconfig.get_config_var('LIBDIR')
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:214:import distutils.sysconfig
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:215:c = distutils.sysconfig.get_config_vars()
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:234:			  "from distutils.sysconfig import get_python_lib as f; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:255:		PYTHON_SITE_PKG=`$PYTHON -c "import distutils.sysconfig; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:256:			print (distutils.sysconfig.get_python_lib(0,0));"`
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:266:	   PYTHON_EXTRA_LIBS=`$PYTHON -c "import distutils.sysconfig; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:267:                conf = distutils.sysconfig.get_config_var; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:278:		PYTHON_EXTRA_LDFLAGS=`$PYTHON -c "import distutils.sysconfig; \
/home/cartheur/github/sphinxbase/m4/ax_python_devel.m4:279:			conf = distutils.sysconfig.get_config_var; \
```
