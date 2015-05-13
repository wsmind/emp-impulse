# Basic Usage #

Before we dive more deeply in the concepts involved in the EMP build system, it's worth looking at a few examples to get a basic understanding of how things work together.

Because the build system is entirely based on [SCons](http://www.scons.org), the main command to start a build is:
```
scons
```
This command should be executed in the root directory of the working copy, where the SConstruct file lives.

To clean a build, you can use the -c option:
```
scons -c
```

The build system often need parameters called variables (**not** environment variables), either to find things or to move aside from default behaviour. The most straightforward way to specify variables is on the command-line:
```
scons DEBUG=true QTDIR=/my/qt/install
```

But because specifying them every time on the command-line is a bit tedious, you can also put them in a file called localconfig.py in the same folder as the SConstruct file. The magic about this file is that it is a python script, thus allowing you to do whatever is necessary to initialize variables. For instance, the following code snippet is valid content for localconfig.py:
```
import os

if os.name == "nt":
    QTDIR = "C:\\Qt\\Magic"

# Under other systems, the variable would keep its default value
```

If you want to list all possible variables along with their documentation and current value, the --help command of scons is overridden for that:
```
scons --help
```

For more advanced scons usage, it is highly recommended that you read through the [SCons user guide](http://scons.org/doc/production/HTML/scons-user/index.html).

# Components #

From a more abstract point of view, the build system is completely designed around the concept of **component**, so we will start reviewing now what that big furry beast is.

A component is a generalized abstraction of an IDE project. But also of an external library, an internal library, an executable, or a compiled pack of resources. In other words, a component is just a _coarse-grained architecture element_. Bigger than one source file, but smaller than the whole game.

We chose to implement a build component model because components are actually the way we **think**. For example, it is very common to think something like "I'm writing a game using Lua, SDL and a home-made UI library over SDL". This sentence would mentally translate to the following graph (internal components in blue, external components in yellow):

![http://yuml.me/7a17ea47.png](http://yuml.me/7a17ea47.png)

Now let's review how this graph would be implemented in _real life_. The game project would have to be configured to use the include paths, linker options and possibly some defines for the three libraries Lua, SDL and UI. Additionally, its artifacts (i.e .cpp files) would also have to be specified somewhere.

Similarly, the UI library should be configured to output a library, as opposed to the game executable, and would receive the same SDL configuration as the game (include paths etc.).

Finally, the SDL and Lua libraries would have to be downloaded and installed, in a consistent manner with the aforementioned _include paths_.

Obviously, these things will still have to be done for the compiler to run, component model or not. The major differences with the component approach is _when_ and _where_ these things get done. For instance, we can see that even with such a small example, the configuration work for using SDL has to be duplicated between the game project and the UI library. Maybe, we could try to factor out this work and centralize it in only one place, so that every new module that needs to use SDL have access to this configuration immediately.

This design is actually the base of [pkg-config](http://pkg-config.freedesktop.org/wiki/), which we _could_ have used. Unfortunately, pkg-config is poorly cross-platform and configuration files are tied to one tool chain, so we rewrote this behaviour on top of scons, and we use pkg-config data whenever available, mostly on Linux platform.

A component is defined as a coherent set of the following data:
  * Unique name
  * Dependency list (of other component names)
  * Artifact description
  * Usage description

## Unique name ##

Because the component name act as an ID, it has to be unique. This allows for simple and readable inter-component referencing.

## Dependencies ##

Each component gives a list of other components it _needs_. In our example, the game would have the 3 libraries in its dependency list, and the UI library would only have the SDL here.

## Artifacts ##

Internal components declare _artifacts_ to build, which are the various outputs that the build system must produce to have a built version of that component. For instance, executables and libraries are artifacts. External components generally don't declare artifacts because they are pre-built, but we could consider using a source external component which would be built at the same time as other components requiring it.

## Usage ##

Every component that can end up in the dependency list of other components must declare a _usage_ which is the configuration set to apply when this component gets _used_. For instance, an external library may add an include path and a define to the compiler command-line. And because scons has great abstractions, this can even be done in a cross-compiler way.

# Actual implementation #

Note: it is highly recommended that you know how to use [SCons](http://www.scons.org) before playing around with the implementation.

The build system implementation is organized in a few files, all at the repository root:
  * **SConstruct** is the entry point for scons. It calls other parts of the build system after having defined a base build environment.
  * **build.py** implements the component approach, as well as the dependency graph traversal. It also stores the _Component_ interface, that every component will have to implement.
  * **components.xxx.py** files contain actual component definitions. There can be as many as this files as necessary, the 'xxx' part being a simple way of categorizing them.

## Component classes ##

Because scons is completely based on python, component files are invoked as SConscripts and are thus python files too. Components are implemented as python classes subclassing build.Component. Once defined, they must be declared to the only object shared with component files: the _dependency walker_.

The general syntax of a component is as follows:
```
class cat_ui(build.Component):
	"""A UI library for devices targeted towards the cat market"""
	
	def __init__(self):
		# The two constructor parameters are component name and dependency list
		build.Component.__init__(self, "cat-ui", ["qtcat", "meoww"])
	
	def appendArtifacts(self, env):
		env.AppendUnique(CPPDEFINES = ["BUILDING_CAT_UI"])
		sources = env.Glob('build/src/cat/ui/*.cpp')
	        if env["OSNAME"] == "nt":
                    sources += env.Glob('build/src/cat/ui/win32/*.cpp')
		env.SharedLibrary('lib/' + self.name, sources)
	
	def appendUsage(self, env):
		env.AppendUnique(LIBS = ["cat-ui"])

# declare component to the dependency walker
walker.declareComponent(cat_ui())
```

An important point is that when a scons environment is given, it is used both for **input** (knowledge about target and configuration) and **output** (declaring artifacts or usage). To see exactly what is already in this SCons environment when a component receives it, you have to look at SConstruct (for the base environment) and other component usages.

# Component graph extraction #

Because with this implementation, the tools **know** what the dependencies between components are, it is possible to generate a graph showing them. This is done automatically, using dot, and can be disable with the DISABLE\_GRAPH configuration variable. A real-life example of such a graph can be found on the [front page of Impulse reference documentation](http://build.emp.fr.nf/r/impulse-testing/doc/)

# About the build server #

A build server has been connected to the repositories by the [WebHooks](http://code.google.com/p/support/wiki/PostCommitWebHooks) provided by Google Code. That means that every time someone pushes something on a repository, the project gets instantly rebuilt and a report is generated. If the build succeeds, the documentation is generated (doxygen) and automated unit tests are run.

The build server lives at address http://build.emp.fr.nf, where you can check the latest builds done for the project, along with a detailed report of command outputs.