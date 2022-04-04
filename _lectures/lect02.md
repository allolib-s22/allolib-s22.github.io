---
num: "Lecture 02"
lecture_date: 2022-04-04
desc: ""
ready: false
---


# Understanding Allolib Playground

Given that the `allolib-playground` repo has, as it's goal, to make allolib easier to understand for beginners, 
there is still quite a bit about the `allolib-playground` repo that is quite difficult to understand.

And even after having taught this course once, there is still quite a lot that I haven't yet managed to grok.


# The `./run.sh` script

At present, the only way that any of us knows how to build an Alloib program is to put it in a single C++ source file, and
then use the `./run.sh` script.

**This is clearly not ideal.**

But to move beyond this, someone with a pretty deep knowledge of CMake will need to figure out the complex structure of `allolib-playground`, and the `run.sh` script.

What makes it even more challenging is that the `./run.sh` script is designed to paper over differences among multiple platforms:
* Windows (with three different hard coded versions of Visual Studio)
* Mac
* Linux

So any attempt to deconstruct what is going on there has to account for at least five different computing environments across three different platforms.

Therefore, as much as it may feel like a straightjacket, for now, we are pretty much restricted to working in a single source file; though it is possible to use `#include` to do at least a little bit of modularization and code reuse.

# Understanding the examples

In the `allolib-playground` repo, almost everything that will be of interest is under the `tutorials` directory.

Further, under `tutorials`, we find this:

<img width="313" alt="image" src="https://user-images.githubusercontent.com/1119017/161457775-5aa1bffe-4c6f-42be-9688-dd82f7d77b0a.png">

We'll be concentrating mostly in the `synthesis` directory for now.

# A simpler example

While the `01_SineEnv.cpp` file is offered as the sort of "Hello World" of `allolib-playground`, it is already way too complicated to really play that role well.  So, let's look at a simpler file, where we try to simplify, simplify, simplify.


