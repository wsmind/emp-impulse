This page gives a rough outline of the tools we will be using on this project and how to install them and set them up on your environment. More detailed documentation can be found in other pages of the [developer manual](DevManual.md).

# Target systems #
The guide below is supposed to work on the following platforms:
  * Linux (Ubuntu, Debian Lenny)
  * Windows (XP, Vista, 7)

If you encounter troubles while following instructions, please leave a comment on this page.

# Fetching from repositories #

We are using [Mercurial](http://mercurial.selenic.com/) on this project. Start by downloading and installing if you are on windows. A debian package is available on standard repositories for Ubuntu and Debian.

Once installed, it should be in your PATH automatically. To check this is the case, open a terminal and run 'hg' without argument. You should see something like this:
```
Mercurial Distributed SCM

basic commands:

 add        add the specified files on the next commit
 annotate   show changeset information by line for each file
 clone      make a copy of an existing repository
 commit     commit the specified files or all outstanding changes
 diff       diff repository (or selected files)
 export     dump the header and diffs for one or more changesets
 forget     forget the specified files on the next commit
 init       create a new repository in the given directory
 log        show revision history of entire repository or files
 merge      merge working directory with another revision
 pull       pull changes from the specified source
 push       push changes to the specified destination
 remove     remove the specified files on the next commit
 serve      export the repository via HTTP
 status     show changed files in the working directory
 summary    summarize working directory state
 update     update working directory

use "hg help" for the full list of commands or "hg -v" for details
```

You can then go to your desired folder for pulling the repository and run the command given [here](http://code.google.com/p/emp-impulse/source/checkout?repo=testing) to get a copy of the testing repository.

**Important Note: you should not push anything but bug fixes on this repository. New features go elsewhere and you will have to clone other repositories before working on them** (see [the workflow page](DevWorkflow.md) for more information).

# Building the project #

Now you have a local clone of the project, you can built it. Here is the basic tools you need to accomplish this goal.

## A C++ compiler ##
Ok that one was obvious. The toolchain described here supports Linux/gcc, MinGW/gcc and MSVC++9.

## Python (>= 2.6) ##
To install Python on windows, you have to download it from the [official website](http://www.python.org/download/) and follow the instructions. Make sure you add the python executable to your PATH after the installation.

Under Ubuntu, you just have to apt-get python...

On Debian/Lenny, the repository package is Python2.5, so you have to install 2.6 from source. A quick and working explanation (for x86 and x64) is given [here](http://nomo17k.wordpress.com/2010/03/13/installing-python-2-6-from-source-on-debian-lenny-amd64/). To avoid version conflicts with other old Python apps, I renamed /usr/local/python and always invoke python2.6 explicitly when I need this version.

## SCons ##
You can download the Windows version of SCons from the [official website](http://www.scons.org). The installer should find your python install and add scons scripts there. Nevertheless you must also add <python dir>/Scripts to your PATH, so that scons can be invoked directly from the command-line.

On Linux, just apt-get scons.

## Project-specific dependencies ##
To ease developer life, we made a small package containing pre-build libraries for windows that we will be using on this project. It is available in the project download page.

Under Ubuntu, you have to apt-get libsfml-dev libboost-dev liblua5.1-0-dev. Under debian, you have to install sfml 1.5 from the archive distributed on the official website, lua development package from the official stable repository and boost from the backports repository.

## Testing your install ##

Once everything is installed, you can go to the root of your working copy of the testing repository, and run 'scons'. If everything is ok, you should see something similar to what appears in [the last automatic build report](http://build.emp.fr.nf/r/impulse-testing).

Note that under Windows, you will have to give the DEPENDENCY\_PACK\_HOME variable on the command-line to point to the folder where you extracted the dependency pack. More information about build variables can be found on the [build system page](http://code.google.com/p/emp-impulse/wiki/DevBuild).

# Running things #

If you want to run something, like the cheese example, you have to use the run-local-binary.py script. This is because of the locality of dynamic libraries. In production environment, dynamic libraries (.so and .dll) are in system paths and will be found automatically. When you want to use them right after building them without having to copy them to system paths, environment variables (e.g LD\_LIBRARY\_PATH) can be used to point to the local lib/ directory.

The system-dependent configuration has been hidden in a python script and thus you must run
```
python run-local-binary.py bin/test/examples-cheese
```

instead of the following form which _might_ not work, depending on the executable and the library it requires
```
cd bin/test && ./examples-cheese
```

# Test data #

To run interesting tests, you will also need testing assets from the FTP. The paths are consistent so you just have to download the folder "test-data" from the FTP and put it in the folder containing your local repositori(es).