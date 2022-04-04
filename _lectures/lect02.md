---
num: "Lecture 02"
lecture_date: 2022-04-04
desc: ""
ready: false
---


# Wednesday: Questions for Andres Cabrera


> Question: I understand that the ./run.sh script is there as an convenient to quickly get started with allolib-playground for "single source file C++ applications".
> 
> But suppose I wanted to develop a more complex application, in a manner that is more the norm among experienced C++ software developers.
> 
> That would typically involved developing a series of C++ classes in the "normal way" i.e.
> 
> The usual practice is to try to develop reusable C++ classes, used code across multiple applications, and with keeping each application main program as small as possible, with only the elements that are unique to each main programming
> * Each class has it's  API specified is in it's own individual .h or .hpp file
> * Each class implementations is placed in its own .cpp file 
> * etc.
> Is there a good example of how to do that?  Or is that unexplored territory?

> Related:
> 
> A typical way to develop with C++ libraries is to create a .a file (or a series of .a files) and then provide a "binary release" of the .a files that is installed into directories such as /include /lib /man etc. that can either be system directories, or better yet, user specified directories to which an individual make file can be pointed.
>
> In this way, the users' repo is small, and clean, and has just their own application code, but a binary can still be built that will run.
>
> Is there any existing configuration of the allolib-playground ecosystem that has this form factor?
>
> If not, is there any interest in doing so?  Any barrier to doing so?  Any reason why doing so would be counterproductive or infeasible to maintain?

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


This simpler example can be found here: 

* <https://github.com/allolib-s22/lab01-pconrad/blob/main/tutorials/allolib-s22/010_SimpleSineEnv.cpp>

An even simpler example appears here.  This one leaves out the graphics part (while leaving the GUI controls in place):

* <https://github.com/allolib-s22/lab01-pconrad/blob/main/tutorials/allolib-s22/005_SimplerSineEnv.cpp>
