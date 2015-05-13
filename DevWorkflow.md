# Introduction #

Because we have chosen to make real use of [Mercurial](http://www.hginit.com) on this project, we don't have only one central repository. Different project versions live at the same time on different repositories, involving a lot of branching and merging.

This page aims at describing who does what on which repositories, and when. If you haven't read it yet, you may look at the [component life cycle](DevComponent.md), which is a good introduction to this document.

# Roles #

When interacting with the project, people will have different roles, and will probably cumulate them. We can identify four major roles:

  * The **user**, which expects a great fun game with regular stable releases.
  * Because users don't like bugs, we also need **testers**, who will find bugs before the game reaches the gamer.
  * The **developers** (official commiters) will be responsible for writing the game code, reviewing each other's code, and fixing bugs the testers find.
  * **Community devs**, who may want to add features or help with bugfixes can clone the project repositories and push additions to it. Their changes can then be reviewed by the core team to discuss their integration.

To allow different overlapping sets of people to work on different things (e.g developing a new feature while fixing bugs of another), we need to separate places where different work is done.

# Repositories #

While the project is divided into a number of repositories, it is important to keep in mind that a repository is _just_ another version of the game, with different parts written, tested, released, etc.

We make use of the following repositories:
  * **stable** stores the official game releases. It is thus the least frequently updated repository. The QA is responsible for pushing releases to this repository.
  * **testing** is the playground of the QA. Testers search for bugs in this repository, and bugs get fixed here too.
  * **dev** contains actively developed, peer-reviewed and unit-tested components. The features under development here have not yet been submitted to QA.
  * **user-clones** will be created by community developers willing to help on the project. Official commiters may also clone the project for use with a very small subteam, or to version a small experiment online.

# Big picture #

As a good schema will probably help a lot more than everything that could be written, here it is.

![https://docs.google.com/drawings/pub?id=1hSKY2gTPBlzL4UWk_ilO0TXpkowVyqAbWLj86j30Jmg&w=900&h=442&wikirequiresextension=.png](https://docs.google.com/drawings/pub?id=1hSKY2gTPBlzL4UWk_ilO0TXpkowVyqAbWLj86j30Jmg&w=900&h=442&wikirequiresextension=.png)

# About sandbox #

There is another free-standing repository called **sandbox**, where you can put all you small experiments that are not directly related to project code (e.g library tests). This repository is not cloned from any other repository, and is never supposed to be pushed or pulled from other repositories.