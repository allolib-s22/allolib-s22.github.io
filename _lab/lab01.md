---
desc: Getting Started
assigned: 2021-03-29 12:30
due: 2021-04-02 23:59
layout: lab
num: lab00
ready: false
signup_app: https://ucsb-cs-github-linker.herokuapp.com/
slack: https://allolib-s21.slack.com
course_org: https://github.com/allolib-s21
course_org_name: allolib-s21
starter_repo: https://github.com/AlloSphere-Research-Group/allolib_playground
prefix: demo1
---


# STILL A WORK IN PROGRESS... PLEASE IGNORE FOR NOW

This assignment is `lab01`

If you find typos or problems with the lab instructions, please report
them on the `#help-lab01` channel on Slack

# Goals

This lab checks that you can succesfully create a piece using the techniques shown in the file `tutorials/01_SineEnv.cpp` in the repo <{{page.starter_repo}}>.


# The Short Way 

If you are already very familiar with GitHub, this way should be easier and faster than the instructions below.

The idea is to get your working copy of Allolib up on GitHub where it can be shared with others in the course.

1. Find the place where you already cloned the Allosphere Playground repo.
2. In the github organization allolib-s21, where you should be a member, you should find a repo `demo1-cgaucho` where `cgaucho` is your username.
3. Add this repo as a remote called `demo1` with the command below (replacing `cgaucho` with your github id)
   ```
   git remote add demo1 git@github.com:allolib-s21/demo1-cgaucho.git
   ```
4. Do a `git checkout -b main` then do a `git push demo1 main` to publish the contents of this repo into your demo1 repo
5. Now, you should be able to publish new branches and or changes as needed.


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


## Step 5: Configure ssh keys for  your machine for git/GitHub


It's convenient  to be able to use `git` and GitHub with ssh links, so for that we need to set up public-key/private-key pairs.

We also want to set up `git` so that it records our commits properly.   Note that some of the descriptions below are 
oriented towards "CSIL"; you may need to adapt these to apply to your own machine.

1. `git` configuration: [Detailed Instructions](https://ucsb-cs156.github.io/topics/csil_git_configuration/)

2.  Configure your machine's ssh keys for git
    - Detailed instructions: [Configuring your ssh key for Github.com](https://ucsb-cs156.github.io/topics/github_ssh_keys/)

3.  If you are brand new to git and github, review a few basic facts about git and github.com 
    - detailed information [here](https://ucsb-cs156.github.io/topics/git_overview/)

## Step 6: Finding your demo1- repo on GitHub

Open a web browser and login to GitHub, then navigate to the course organization page, <{{page.course_org}}>.

You should see that there is a private repo in this organization called `demo1-yourGithubId`, where `yourGithubId` is replaced with your GitHub id.  This is the repo
that you'll be using for this assignment.

This is currently an empty repo.  In the next step, we'll clone this empty repo into a directory on your local system.

## Step 7: Cloning the repo


1. Login to your CSIL account,  create a `~/cs156` subdirectory, and change directory into it
   
   ```
   mkdir ~/allolib
   cd ~/allolib
   ```

   (To be honest, you can actually use any directory you like; but we suggest this approach.  We'll refer
   to `~/allolib` throughout the rest of
   the instructions for consistency.

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


## Step 9: A remote for starter

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

Then, use this command to add a remote called `starter` for the starter code repo:

```
git remote add starter paste-url-here
```

After this command, use `git remote -v` to list all your remotes. Your output should look like this (except your GitHub id in place of `cgaucho`):

<tt>
% git remote -v<br />
origin	git@github.com:{{page.course_org_name}}/{{page.num}}-cgaucho.git (fetch)<br />
origin	git@github.com:{{page.course_org_name}}/{{page.num}}-cgaucho.git (push)<br />
starter	git@github.com:AlloSphere-Research-Group/allolib_playground.git (fetch)<br />
starter	git@github.com:AlloSphere-Research-Group/allolib_playground.git (push)<br />
% 
</tt>

## Step 10: Pull Starter Code into your Repo

The next step is to pull the starter code into your repo, and then push
that code to your origin repo on GitHub.

Here are the three commands:

```
git checkout -b main
git pull starter main
git push origin main
```

After these three commands, go look at your repo on GitHub, i.e. the repo at this url (but substituting your GitHub id for cgaucho:)

* <https://github.com/{{page.course_org_name}}/{{page.prefix}}-cgaucho>

You should see that instead of an empty repo, you now have a copy of the starter code.


## Step 11: Compile and run the Starter code

Next, you'll want to go through the steps needed to get the repo set up (e.g. the steps in `USING.md`)


