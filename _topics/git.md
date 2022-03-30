---
topic: git
description: "version control system"
---


Using git to add a remote for Allolib Playground


```
Last login: Tue Apr  6 20:52:53 on ttys002
pconrad@Phillips-MacBook-Pro ~ % cd github
pconrad@Phillips-MacBook-Pro github % cd allolib-s22               
pconrad@Phillips-MacBook-Pro allolib-s22 % git clone https://github.com/allolib-s22/demo1-pconrad
Cloning into 'demo1-pconrad'...
warning: You appear to have cloned an empty repository.
pconrad@Phillips-MacBook-Pro allolib-s22 % cd demo1-pconrad 
pconrad@Phillips-MacBook-Pro demo1-pconrad % git remote add ap git@github.com:AlloSphere-Research-Group/allolib_playground.git
pconrad@Phillips-MacBook-Pro demo1-pconrad % git checkout -b master
Switched to a new branch 'master'
pconrad@Phillips-MacBook-Pro demo1-pconrad % git pull ap master
remote: Enumerating objects: 350, done.
remote: Counting objects: 100% (350/350), done.
remote: Compressing objects: 100% (239/239), done.
remote: Total 623 (delta 182), reused 244 (delta 95), pack-reused 273
Receiving objects: 100% (623/623), 1.34 MiB | 3.15 MiB/s, done.
Resolving deltas: 100% (311/311), done.
From github.com:AlloSphere-Research-Group/allolib_playground
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> ap/master
pconrad@Phillips-MacBook-Pro demo1-pconrad % 
pconrad@Phillips-MacBook-Pro demo1-pconrad % git push origin master
Enumerating objects: 623, done.
Counting objects: 100% (623/623), done.
Delta compression using up to 16 threads
Compressing objects: 100% (282/282), done.
Writing objects: 100% (623/623), 1.34 MiB | 753.00 KiB/s, done.
Total 623 (delta 311), reused 623 (delta 311), pack-reused 0
remote: Resolving deltas: 100% (311/311), done.
To https://github.com/allolib-s22/demo1-pconrad
 * [new branch]      master -> master
pconrad@Phillips-MacBook-Pro demo1-pconrad % 

```
