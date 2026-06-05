---
title: "Use many different Git accounts automatically"
description: "As I have developer accounts for GitHub, Gitlab and Codeberg, I want to choose them automatically, when I commit"
date: 2026-06-05
publishDate: 2025-07-18 
lastmod: 2026-06-05
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

[includeIf "gitdir:~/dev/projects/private/codeberg/private/"]
path = /home/joergi/.gitconfig-codeberg-private

[includeIf "gitdir:~/dev/projects/private/codeberg/super-private/"]
path = /home/joergi/.gitconfig-codeberg-super-private
```
> **Edit:** it's super important that the `includeIf` ends with a `/`   
> Else it will not work. With `/**` it will match all repos underneath. That's exactly what I want!  

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

> **Edit** In the beginning I had the code written like this for each codeberg one (only showing one for simplicity)
```shell
Host codeberg.org
    HostName codeberg.org
    User git
    IdentityFile ~/.ssh/id_codeberg_private
    IdentitiesOnly yes
```
> But that was the error.
> The only thing needed is:
```shell
Host codeberg.org
    HostName codeberg.org
    User git
```
> no identity file, nothing!

Something I really found out at the end of the configuration journey, that you need to configure everything in the `.ssh/config`
> **Edit** so this is the update one `.ssh/config` file:
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
```

## Solved problems
If you are on a corporate computer, it can happen that some outgoing ssh connections are blocked. I had to ask our admins to unblock the specific codeberg.org server.   
This costed my hours, because github and gitlab were perfectly working and I assumed that I set up something incorrectly in my environment, as codeberg was the only one server where I had 2 different git accounts.  
But at the end it was AGAIN the corporate firewall/security setting.  
This was really frustrating. So if you have problems, first check if it's the corporate setting!

> **Edit** The problem with the not knowing which git user it should use for codeberg is with the new edit above solved, hurray.

## ToDo: signed commits for all git accounts
As you can see, only the Github account has so far signed commits. This is something I also have to do for my other Git accounts.
