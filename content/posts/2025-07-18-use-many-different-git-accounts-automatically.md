---
title: "Use many different Git accounts automatically"
description: "As I have developer accounts for GitHub, Gitlab and Codeberg, I want to choose them automatically, when I commit"
date: 2025-07-18
---

## Preparing the folder structure for using new setup   
At the moment I have different git accounts.  
I have one for Github (work & private), Gitlab and two accounts for Codeberg (private and a even more private one).  
To get this running I had to prepare my folders a little bit. before I only had everything in two folders, private and work, but it all used the same GitHub account.

```shell
/home/joergi/dev/projects/
├─ privat/
│  ├─ codeberg/
│  │  ├─ private/
│  │  │  ├─ project1/
│  │  │  ├─ project2/
│  │  ├─ more-private/
│  │  │  ├─ projectA/
│  │  │  ├─ projectB/
│  ├─ gitlab/
│  │  ├─ projectGL1/
│  │  ├─ projectGL2/
│  ├─ github/
│  │  ├─ projectPrivatGH1/
│  │  ├─ projectPrivatGH2/
├─ work
│  ├─ github
```

When I only used Github, my `.gitconfig` looked like this:
```shell
[user]
	email = 1234567879+username@users.noreply.github.com
	name = joergi
	signingkey = ABCDE123456789
[commit]
	gpgsign = true
[core]
	autocrlf = input
```
## Creating ssh keys for each git account
You have to create for each project you want to use a ssh key.  
I follow here the instructions from the [GitHub documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).  
And I add the ssh to [Codeberg](https://docs.codeberg.org/security/ssh-key/), [Gitlab](https://docs.gitlab.com/user/ssh/) and [Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).
And of course I rename the ssh files, so I can recognize them later:
```shell
-rw-------  1 joergi joergi  432 Apr 25 16:02 id_codeberg_more_private
-rw-r--r--  1 joergi joergi  118 Apr 25 16:02 id_codeberg_more_private.pub
-rw-------  1 joergi joergi  419 Jul  4 10:49 id_codeberg_private
-rw-r--r--  1 joergi joergi  107 Jul  4 10:49 id_codeberg_private.pub
-rw-------  1 joergi joergi 2,6K Apr 25 14:00 id_github_rsa
-rw-r--r--  1 joergi joergi  593 Jun 10  2020 id_github_rsa.pub
-rw-------  1 joergi joergi  411 Apr 25 14:23 id_gitlab
-rw-r--r--  1 joergi joergi   97 Apr 25 14:23 id_gitlab.pub
```

## Preparing .gitconfig and custom .gitconfig files
Nowadays my `.gitconfig` looks like this:
```shell
[includeIf "gitdir:~/dev/projects/private/github/"]
path = /home/joergi/.gitconfig-github

[includeIf "gitdir:~/dev/projects/work/github/"]
path = /home/joergi/.gitconfig-github

[includeIf "gitdir:~/dev/projects/private/gitlab/"]
path = /home/joergi/.gitconfig-gitlab

[includeIf "gitdir:~/dev/projects/private/codeberg/private"]
path = /home/joergi/.gitconfig-codeberg-private

[includeIf "gitdir:~/dev/projects/private/codeberg/super-private/"]
path = /home/joergi/.gitconfig-codeberg-super-private
```
As you can already see, we have to define a gitconfig file for each git account.  
At the moment I still use the same GitHub config for work and private, I will change that later.  

The custom gitconfig files are at the same place as the main `.gitconfig` file: 
```shell
/home/joergi/
├─ .gitconfig
├─ .gitconfig-github
├─ .gitconfig-gitlab
├─ .gitconfig-codeberg-private
├─ .gitconfig-codeberg-super-private
```
This is for example the `/home/joergi/.gitconfig-codeberg-private`
```shell
[user]
    email = my-private-codeberg-username@mydomain.de
    name = private-username
[core]
    autocrlf = input
[init]
    defaultBranch = main
```
the other codeberg config file looks like this:
```shell
[user]
    email = my-super-private-codeberg-username@mydomain.de
    name = more-private-username
[core]
    autocrlf = input
[init]
    defaultBranch = main
``` 

and `/home/joergi/.gitconfig-github` still looks the same as when I had only one `.gitconfig` file:
```shell
[user]
	email = 1234567879+username@users.noreply.github.com
	name = joergi
	signingkey = ABCDE123456789
[commit]
	gpgsign = true
[core]
	autocrlf = input
```

Something I really found out at the end of the configuration journey, that you need to configure everything in the `.ssh/config`
```shell
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_github_rsa
    IdentitiesOnly yes
    
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_gitlab
    IdentitiesOnly yes
    
Host codeberg.org
    HostName codeberg.org
    User git
    IdentityFile ~/.ssh/id_codeberg_private
    IdentitiesOnly yes
    
Host codeberg.org
    HostName codeberg.org
    User git
    IdentityFile ~/.ssh/id_codeberg_more_private
    IdentitiesOnly yes

```
## ToDo: signed commits for all git accounts
As you can see, only the Github account has so far signed commits. This is something I also have to do for my other Git accounts.

## Solved problems
If you are on a corporate computer, it can happen that some outgoing ssh connections are blocked. I had to ask our admins to unblock the specific codeberg.org server.   
This costed my hours, because github and gitlab were perfectly working and I assumed that I set up something incorrectly in my environment, as codeberg was the only one server where I had 2 different git accounts.  
But at the end it was AGAIN the corporate firewall/security setting.  
This was really frustrating. So if you have problems, first check if it's the corporate setting!

## Still an unsolved problem
when I create a new repository and I want to commit something, I get the following error message:
```shell
Author identity unknown

*** Please tell me who you are.

Run

git config --global user.email "you@example.com"
git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'joergi@COMPUTERNAME.(none)')
```

So it seems that the git doesn't know the username yet. Before setting the specific repository `.git/config`, it looks like this:
```shell
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://git@codeberg.org/private-username/test.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
	remote = origin
	merge = refs/heads/main
```
So I set the `user.email` and `user.name` but without the `--global flag`
```shell
$ git config user.email "my-private-codeberg-username@mydomain.de"

$ git config user.name "private-username"

```

the `~/dev/projects/joergi/codeberg/private/test/.git/config` looks now like this:
```shell
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
[remote "origin"]
    url = ssh://git@codeberg.org/private-username/test.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
    remote = origin
    merge = refs/heads/main
[user]
    email = my-private-codeberg-username@mydomain.de
    name = private-username
```
This solves the problem, but it's a bit annoying, that I have to do this for every new repository. Still looking for a solution.