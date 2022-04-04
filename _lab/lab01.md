---
desc: Getting Started
assigned: 2021-03-30 12:30
due: 2021-04-06 11:00
layout: lab
num: lab01
ready: false
signup_app: https://ucsb-cs-github-linker.herokuapp.com/
slack: https://allolib-s22.slack.com
course_org: https://github.com/allolib-s22
course_org_name: allolib-s22
starter_repo: https://github.com/AlloSphere-Research-Group/allolib_playground
prefix: lab01
---


This assignment is `lab01`

If you find typos or problems with the lab instructions, please report
them on the `#help-lab01` channel on Slack

# Goals

This lab checks that you can succesfully create a piece using the techniques shown in the file `tutorials/01_SineEnv.cpp` in the repo <{{page.starter_repo}}>.


# The Short Way 

If you are already very familiar with GitHub, this way should be easier and faster than the instructions below.

The idea is to get your working copy of Allolib up on GitHub where it can be shared with others in the course.

1. In the github organization allolib-s22, where you should be a member, you should find a repo <tt>{{page.prefix}}-cgaucho</tt> where `cgaucho` is your username.
2. Clone this repo to your own machine.
3. Add a remote for the allolib-playground repo:
   ```
   git remote add allolib-playground git@github.com:AlloSphere-Research-Group/allolib_playground.git
   ```
4. Do these commands to get a copy of `allolib-playground/master` into your local repo as
   the `origin/main` branch:
   
   ```
   git checkout -b master
   git pull allolib-playground master
   git checkout -b main
   git push origin main
   ```

5. Run the command inside `init.sh`:

   ```
   git submodule update --init --recursive
   ```

6. Now, you should be able to publish new branches and or changes as needed.

Then: 
* Set up the preliminaries needed for Allolib: Follow the instructions here for your platform: <https://github.com/AlloSphere-Research-Group/allolib/blob/master/readme.md>
* Follow the instructions for getting started with Allolib-Playground: <https://github.com/AlloSphere-Research-Group/allolib_playground>
* Try running the Sine Wave demo:
  ```
  ./run.sh tutorials/synthesis/01_SineEnv.cpp
  ```

If it works, you are done!  You can demo during class to show the instructor that
you were successful.

# The Long Way



## Step 1: Get setup with github and add yourself to our organization

We will be using github.com in this course. We have created an
organization called {{site.github_org}} on github.com where you can
create repositories (repos) for your assignments in this course.

The advantage of creating private repos under this organization is
that the instructor, helpers, and fellow students in the course will be able to see
your code and provide you with help, without you having to do anything
special.

To join this organization, you need to do three things.

1. If you don't already have a github.com account, create one on the
"free" plan. Visit [https://github.com/](https://github.com/)

2. If you don't already have your `@ucsb.edu` email address
associated with your github.com account. go to "settings", add that
email, and confirm that email address.  (If you have an `@umail.ucsb.edu` address registered there, that also works.)

3. Visit our Github Sign Up Tool at <{{page.signup_app}}>.   Navigate to <{{page.signup_app}}>.  Login with your github.com account. Click "Home", find this course ({{site.course}}, {{site.qtr}}), and click the "Join course button".   That will automatically send you an invitation to join the course organization on github.

4. There should be a link to the invitation for the GitHub organization for this course (<https://github.com/{{site.github_org}}>). Click on the invitation link and accept it. You can also go straight to <https://github.com/{{site.github_org}}> and see the invitation there (if you're logged in). Accept the invitation that appears in your browser (from step 3) or log into your account on [https://github.com/](https://github.com/) to accept the invitation.

Note: if you have some reason why you strongly prefer not to associate your UCSB email address with your GitHub account, you may request to be added to the organization manually.
In this case, please let Phill know your GitHub id.


## Step 2: Configure ssh keys for  your machine for git/GitHub


It's convenient  to be able to use `git` and GitHub with ssh links, so for that we need to set up public-key/private-key pairs.

We also want to set up `git` so that it records our commits properly.   Note that some of the descriptions below are 
oriented towards "CSIL"; you may need to adapt these to apply to your own machine.

1. `git` configuration: [Detailed Instructions](https://ucsb-cs156.github.io/topics/csil_git_configuration/)

2.  Configure your machine's ssh keys for git
    - Detailed instructions: [Configuring your ssh key for Github.com](https://ucsb-cs156.github.io/topics/github_ssh_keys/)

3.  If you are brand new to git and github, review a few basic facts about git and github.com 
    - detailed information [here](https://ucsb-cs156.github.io/topics/git_overview/)

## Step 3: Finding your <tt>{{page.prefix}}</tt> repo on GitHub

Open a web browser and login to GitHub, then navigate to the course organization page, <{{page.course_org}}>.

You should see that there is a private repo in this organization called 
<tt>{{page.prefix}}-cgaucho</tt> where `cgaucho` is your GitHub id

That is the repo that you'll be using for this assignment.

This is currently an empty repo.  In the next step, we'll clone this empty repo into a directory on your local system.

## Step 3: Cloning the repo


1. Create a directory somewhere on your machine for your work this quarter, and change directory into it.  You might called it `allolib` for example, but the name is up to you.
   
   ```
   mkdir ~/allolib
   cd ~/allolib
   ```

   (As mentioned above, you can actually use any directory you like; but we suggest this approach.  We'll refer
   to `~/allolib` throughout the rest of
   the instructions for consistency.)

2. Now, go to the `github.com` web page, and find your `demo1-userid` repo. The page should look something like this:

   ![jpa00-cgaucho-50.png](jpa00-cgaucho-50.png)

   You should see a button for `SSH`;
   select that button.  Then there is a button to copy the URL shown;
   click that to copy the URL.

3. Now type this command, replacing
   `url` with the url that you copied.

   That `url` should be something like
   <tt>git@github.com:{{page.course_org_name}}/{{page.num}}-cgaucho.git</tt> but with your GitHub id in place of <tt>cgaucho</tt>.

   ```
   git clone url
   ```

   You'll will see a warning message that you are cloning an empty repo; that's normal.

   
   <tt>Cloning into {{page.prefix}}-cgaucho...<br />
   warning: You appear to have cloned an empty repository<br /></tt>
   
  
4. If you use the `ls` command, you should now have a subdirectory called <tt>{{page.prefix}}-cgaucho</tt> (except <tt>cgaucho</tt> will be your GitHub username.)  Use
   a `cd` command to change directory 
   into that directory, e.g.

   
   <tt>cd {{page.prefix}}-cgaucho</tt>
   

   An `ls -a` should reveal an empty
   directory except for the `.git` subdirectory indicating that this is a GitHub repo.  

   ```
   % ls -a
   .	..	.git
   % 
   ```

   We are now ready to pull in some starter code.


## Step 4: A remote for the starter code (allolib_playground)

The Allolib Playground repo is here: <{{page.starter_repo}}>

If you've used `git` before, you
may be familiar with the command:

```
git pull origin master
```

The word `origin` in this case refers to a *remote*, that is a repo that lives somewhere out there on the network.   

The word `master` refers to the default branch of the repo.  The default branch of GitHub repos recently changed from `master` to `main`; we'll be using `main` throughout this course.

If you type the following command, you'll see that `origin` is defined as a remote for the repo that you cloned from.  Your output will look similar, except that you'll have your GitHub in place of `cgaucho`:

<tt>
% git remote -v<br />
origin	git@github.com:{{page.course_org_name}}/{{page.prefix}}-cgaucho.git (fetch)<br />
origin	git@github.com:{{page.course_org_name}}/{{page.prefix}}-cgaucho.git (push)<br />
% <br />
</tt>

Now, we are going to add a second remote.  This remote will use the URL for the starter code.

The image below shows how to copy that URL: (1) Click the green `Code` button.  (2) Select `SSH` to choose that as the network protocol for the URL (3) Click the icon to copy the URL to your clipboard.

![starter-ssh-url-50.png](starter-ssh-url-50.png)

Then, use this command to add a remote called `allolib-playground` for the starter code repo:

```
git remote add allolib-playground paste-url-here
```

After this command, use `git remote -v` to list all your remotes. Your output should look like this (except your GitHub id in place of `cgaucho`):

<tt>
% git remote -v<br />
origin	git@github.com:{{page.course_org_name}}/{{page.num}}-cgaucho.git (fetch)<br />
origin	git@github.com:{{page.course_org_name}}/{{page.num}}-cgaucho.git (push)<br />
allolib-playground	git@github.com:AlloSphere-Research-Group/allolib_playground.git (fetch)<br />
allolib-playground	git@github.com:AlloSphere-Research-Group/allolib_playground.git (push)<br />
% 
</tt>

## Step 5: Pull Starter Code into your Repo

The next step is to pull the starter code into your repo, and then push
that code to your origin repo on GitHub.

Here are the three commands:

```
git checkout -b master
git pull allolib-playground master
git checkout -b main
git push origin main
```

After these three commands, go look at your repo on GitHub, i.e. the repo at this url (but substituting your GitHub id for cgaucho:)

* <https://github.com/{{page.course_org_name}}/{{page.prefix}}-cgaucho>

You should see that instead of an empty repo, you now have a copy of the `allolib_playground` starter code.

## Step 6: Populate the submodules

The `allolib-playground` repo points to a collection of other repos that contain software that the `allolib-playground` needs in order to 
function.

There is a command in the file `init.sh` that you should type to initialize the submodules:

```
git submodule update --init --recursive
```

If it works, it will look like this; it may take 5-10 minutes to work, depending on the speed of your internet connection and your computer:

```
pconrad@Phillips-MacBook-Pro lab01-pconrad % git submodule update --init --recursive
Submodule 'al_ext' (https://github.com/AlloSphere-Research-Group/al_ext) registered for path 'al_ext'
Submodule 'allolib' (https://github.com/AlloSphere-Research-Group/allolib) registered for path 'allolib'
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/al_ext'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib'...
Submodule path 'al_ext': checked out 'bb45e9229ada686fca0a916c3877a65afe50b943'
Submodule 'statedistribution/cuttlebone' (https://github.com/kybr/cuttlebone.git) registered for path 'al_ext/statedistribution/cuttlebone'
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/al_ext/statedistribution/cuttlebone'...
remote: Enumerating objects: 66, done.
remote: Counting objects: 100% (66/66), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 66 (delta 44), reused 60 (delta 38), pack-reused 0
Unpacking objects: 100% (66/66), 15.30 KiB | 333.00 KiB/s, done.
From https://github.com/kybr/cuttlebone
 * branch            178dbd4a7f46c984e32c98ad61c5fb35f8ac1579 -> FETCH_HEAD
Submodule path 'al_ext/statedistribution/cuttlebone': checked out '178dbd4a7f46c984e32c98ad61c5fb35f8ac1579'
Submodule path 'allolib': checked out 'ac68b234712c796e8c45c3032b304b727ec6c249'
Submodule 'external/Gamma' (https://github.com/AlloSphere-Research-Group/Gamma.git) registered for path 'allolib/external/Gamma'
Submodule 'external/cpptoml' (https://github.com/skystrife/cpptoml.git) registered for path 'allolib/external/cpptoml'
Submodule 'external/glfw' (https://github.com/glfw/glfw.git) registered for path 'allolib/external/glfw'
Submodule 'external/imgui' (https://github.com/ocornut/imgui.git) registered for path 'allolib/external/imgui'
Submodule 'external/json' (https://github.com/nlohmann/json) registered for path 'allolib/external/json'
Submodule 'external/rtaudio' (https://github.com/thestk/rtaudio.git) registered for path 'allolib/external/rtaudio'
Submodule 'external/rtmidi' (https://github.com/thestk/rtmidi) registered for path 'allolib/external/rtmidi'
Submodule 'external/serial' (https://github.com/wjwwood/serial) registered for path 'allolib/external/serial'
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/Gamma'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/cpptoml'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/glfw'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/imgui'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/json'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/rtaudio'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/rtmidi'...
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/serial'...
Submodule path 'allolib/external/Gamma': checked out 'ad80ed56eb37449378b6852d2f37b908c93e8d3c'
Submodule path 'allolib/external/cpptoml': checked out 'fededad7169e538ca47e11a9ee9251bc361a9a65'
Submodule 'deps/meta-cmake' (https://github.com/meta-toolkit/meta-cmake.git) registered for path 'allolib/external/cpptoml/deps/meta-cmake'
Cloning into '/Users/pconrad/github/allolib-s22/lab01-pconrad/allolib/external/cpptoml/deps/meta-cmake'...
Submodule path 'allolib/external/cpptoml/deps/meta-cmake': checked out 'db7ff976c430a6cb6cbd03ae691be04cfaac0a07'
Submodule path 'allolib/external/glfw': checked out 'b0796109629931b6fa6e449c15a177845256a407'
Submodule path 'allolib/external/imgui': checked out '95c99aaa4be611716093edcb6b01146ab9483f30'
Submodule path 'allolib/external/json': checked out 'db78ac1d7716f56fc9f1b030b715f872f93964e4'
Submodule path 'allolib/external/rtaudio': checked out 'd27f257b4bc827e4152cdc4d69a2e22084204afd'
Submodule path 'allolib/external/rtmidi': checked out 'dc7575db8f3f421264b2ee585bdf097ad1f402c5'
Submodule path 'allolib/external/serial': checked out '683e12d2f6a26c80bfa07f276845be618237ae5b'
pconrad@Phillips-MacBook-Pro lab01-pconrad %
```

## Step 7: Set up Allolib dependencies

Then: 
* Set up the preliminaries needed for Allolib: Follow the instructions here for your platform: <https://github.com/AlloSphere-Research-Group/allolib/blob/master/readme.md>
* Follow the instructions for getting started with Allolib-Playground: <https://github.com/AlloSphere-Research-Group/allolib_playground>
* Try running the Sine Wave demo:
  ```
  ./run.sh tutorials/synthesis/01_SineEnv.cpp
  ```


