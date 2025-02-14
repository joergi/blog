---
title: "Getting a verified user on Github"
description: "This is a step by step example what you have to do, to get a verified user on Github"
categories: [linux, developer, github]
tags: [github, verified user, authentication]
date: 2020-03-27
---

When you create a new file directly on Github and push it to your branch, you will see in the commit, that this was done by an verified user.
![Alt text](/img/github1-2.png "Github showing verified user")

If you push it from your command line, it normally looks like this:

![Alt text](/img/github2.png "github 2")

P.S. if you followed the tutorial, and something went wrong, it will look like this:  
![Alt text](/img/github3_fail.png "github showing unverified commit")
    
But let's start from the beginning!    
First of all: 
Github gives you a great Readme for this, that's why I link it here were needed.

1. Add a [new Github GPG](https://help.github.com/en/github/authenticating-to-github/generating-a-new-gpg-key) key [here](https://github.com/settings/gpg/new)
I used my `1234567890+your_github_username@users.noreply.github.com` as my email is protected on Github 
You can always change the email of your [key here](https://help.github.com/en/github/authenticating-to-github/associating-an-email-with-your-gpg-key)
2. You should tell your git to use it, so you have to sign it as [described here](https://help.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key)
3. Sign every commit as mentioned [here](https://help.github.com/en/github/authenticating-to-github/signing-commits)
4. When you make now a commit, you will see that the commit is signed.

At the end, it ~should~ looks like this:
![Alt text](/img/github_verified-1.png "github showing a verified commit")
I tried this tutorial on my other computer, and it all worked, if you have questions, let me know.
