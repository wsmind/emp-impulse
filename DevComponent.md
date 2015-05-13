# Introduction #

Because the project is not so small (TODO: add link to architecture), it is divided into a number of components. This page describe the life cycle of such components, split up into coarse phases.

# Phases #

The component life starts during architecture elaboration, when we decide that we **need** this component. Its positioning relative to other components is visually exposed through the architecture diagram.

It is shortly followed by a first pass of **requirements**, allowing to understand better why this component is here, what it is supposed to do (from an external point of view), and which main features can be expected from other components using it.

The development starts with a **design** phase, where the big internal and external concepts of the components have to be identified. The goal here is not to end up with a detailed UML diagram of the component, but to find the 2-5 big ideas that will lead to a more detailed design and API.

The design is then iteratively refined and simplified (where possible) through **prototyping**. Prototypes allow to quickly identify bad design aspects and give the developer(s) a better view on the component internal workings.

The prototyping process occurs in dedicated repository clones (see the [workflow](DevWorkflow.md) documentation), to give developers the freedom to experiment and **refine** as long as necessary. Nevertheless, pulls from the testing repository should be done on a regular basis, to ensure that the development clone does not differ too much from the testing version.

After enough iterations have been done on the component, it becomes ready to be **tested** by QA. It is thus pushed on the testing repository, allowing testers to work on it and report bugs. The fixing of these bugs will be done in the testing repository (for closer dev/QA interaction), but nothings blocks the development of new features in the dedicated clone while the bug-fixing process is taking place.

# Summary #

The following diagram summarizes the different component phases, along with the various tools used during them.

![http://docs.google.com/drawings/pub?id=1D3aiC-chX-Q-MSZWJ3w8McB8jczfZ7unE-EquUAE0pY&w=700&h=1060&wikirequiresextension=.png](http://docs.google.com/drawings/pub?id=1D3aiC-chX-Q-MSZWJ3w8McB8jczfZ7unE-EquUAE0pY&w=700&h=1060&wikirequiresextension=.png)