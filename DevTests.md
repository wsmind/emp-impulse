# Introduction #

When checking that new code works, we often need something simple, such as a very basic main() with a few calls here and there to play around with the code that has just been written.

This is exactly the spirit in which the EMP unit testing framework has been designed. It allows both automated and non-automated tests to be written, and the automated framework is extremely lightweight.

In the project repository, tests are written under the test/ directory, grouped by category. Every test is a single .cpp file with a main() inside. When the test can be automated, it name is prefixed by "auto-".

# Non-automated tests #

This kind of test is the simplest one. It is just a main() with whatever you want inside. It fits well visual or audio tests, the kind of tests that cannot be easily automated.

# Automated tests #

Automated tests rely on a very simple library called PEEL (Problem Elusive Examination Library). This library is actually a single header with a few macros in it:
  * CHECK(expression) will assert the given expression and print either a success or a failure depending on expression evaluation.
  * COMPARE\_FLOAT(float1, float2) will compare float with a small error tolerance.

Examples of how to use this library can be found in the [repository](http://code.google.com/p/emp-impulse/source/browse/?repo=testing#hg/test/examples).

The important point is that, by using that library for your automated tests, your test becomes compatible with the build server automatic test execution. This will allow for automatic reporting of test fails whenever someone pushes code that do not pass them.

# Running tests locally #

To check that tests run properly on your own machine, you can use two main strategies:
  * Running any test you want (automated or not) with the following command:
```
python run-local-binary.py bin/test/category-name
```
  * Running all automated tests with the following command:
```
python run-automated-tests.py -h
```

Note that if you forget the -h in the second command, the output will be raw JSON...