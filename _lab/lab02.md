---
desc: Allotemplate
assigned: 2021-03-30 12:30
due: 2021-04-06 11:00
layout: lab
num: lab02
ready: true
slack: https://allolib-s22.slack.com
course_org: https://github.com/allolib-s22
course_org_name: allolib-s22
starter_repo: https://github.com/AlloSphere-Research-Group/allotemplate
starter_repo_ssh: git@github.com:AlloSphere-Research-Group/allotemplate.git
prefix: lab02
---


This assignment is <tt>{{page.num}}</tt>

If you find typos or problems with the lab instructions, please report
them on the `#help-lab02` channel on Slack

# Goals

This lab checks that you can succesfully create a piece using the Allotemplate repo: <{{page.starter_repo}}>.

# Instructions

1. If you navigate to the GitHub org for the course at <{{page.course_org}}>, you should find an empty repo called <tt>{{page.num}}-<i>yourGitHubId</i></tt>, where
<tt><i></i></tt> is your GitHub id (for example <tt>{{page.num}}-cgaucho</tt>.)

2. Put yourself in a directory (folder) that you've chosen as the place to store files for this course.  For example, with these commands:

   ```
   cd
   mkdir -p allolib
   cd allolib
   ```
   
   Explanation: 
   * `cd` means:  "change directory(folder) to your home directory(folder)"
   * `mkdir -p allolib` means: m ke a directory(folder) called `allolib`.  The `-p` means: don't complain if one already exists.
   * `cd allolib` puts you into that directory(folder)
   
   
3. Go to the page for your <tt>{{page.num}}-<i>yourGitHubId</i></tt> repo and get the `ssh` link for that repo.

   Then, clone that into your current directory, and cd into that directory:
   
   ```
   pconrad@Phillips-MacBook-Pro allolib-s22 % git clone git@github.com:allolib-s22/lab02-pconrad.git
   Cloning into 'lab02-pconrad'...
   warning: You appear to have cloned an empty repository.
   pconrad@Phillips-MacBook-Pro allolib-s22 % ls     
   allolib-s22.github.io	lab01-pconrad		lab02-pconrad
   pconrad@Phillips-MacBook-Pro allolib-s22 % cd lab02-pconrad 
   pconrad@Phillips-MacBook-Pro lab02-pconrad % 
   ```
4. Add a remote for the `allotemplate` repo:
   
   <tt>git remote add allotemplate {{page.starter_repo_ssh}}</tt>
   
   ```
   ```
 
   
5. Do these commands to get a copy of `allotemplate/master` into your local repo as
   the `origin/main` branch:
   
   ```
   git checkout -b master
   git pull allotemplate master
   git checkout -b main
   git push origin main
   ```

5. DO NOT run the commands inside `./init.sh`!  These will destroy the git history that you already have, and the remotes you
   created.
   
   Instead, run only these commands.  These may take several minutes to complete, depending on the speed of your internet
   connection:
   
   ```
   git submodule add https://github.com/AlloSphere-Research-Group/allolib.git
   git submodule add https://github.com/AlloSphere-Research-Group/al_ext.git
   git submodule update --recursive --init
   ```







