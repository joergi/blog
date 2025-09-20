---
title: "Migrating from GitHub to Codeberg"
description: "Using if you use the official Codeberg migration tool the authors are not linked. so this script does a git history rewrite to link the authors to their Codeberg profiles."
date: 2025-09-20
---

## Reasons for migration
There are many reasons, why many of the open source projects are migrating from GitHub to Codeberg.  
[GitHub Copilot, the AI from GitHub](https://github.com/copilot/) which is owned by Microsoft, gives us the following reasons:
>Main reason:  
many projects prefer a community‑run, non‑profit, open infrastructure (Codeberg e.V. running Forgejo/Gitea) over GitHub’s proprietary, Microsoft‑controlled platform with increasing commercialization (e.g. AI features, data usage, lock‑in concerns). Privacy, transparency, open governance, and avoiding dependence on a single corporate vendor are the core motivators.

When then asking GitHub Copilot for political reasons, it gave me the following list:
> U.S. jurisdiction and exposure to sanctions or export controls (accounts in sanctioned regions restricted).  
> Past controversy over Microsoft (GitHub owner) contracts with U.S. government agencies (e.g. immigration enforcement) raising ethical concerns for some contributors.  
> DMCA takedown handling and perceived overreach (e.g. high‑profile repository removals) creating fears of centralized control.  
> Risk of sudden access loss for developers in geopolitically sensitive regions.  
> Concern over corporate influence on open tooling versus community governance (Codeberg is a German non profit).  
> Desire for stronger EU privacy and data protection alignment (GDPR emphasis).  
> Objections to use of public code for large scale AI training without explicit opt in.  
> Preference to reduce single vendor dependency and potential policy shifts tied to U.S. politics.

And what GitHub Copilot has not directly mentioned:  
["Microsoft contributes $1 million to Trump’s inauguration fund in 2025" (CNBC)](https://www.cnbc.com/2025/01/09/microsoft-contributes-1-million-to-trumps-inauguration-fund.html)   
  
So, yeah, there are a lot of reasons to migrate.    
So I wanted to migrate some of my repos too and I had a look how this could be done.  

## Official migration tool from Codeberg
If you go to Codeberg, there is an official migration tool:
{{< figure src="/img/codeberg_migration_button.png" caption="Button for doing the migration"  >}}
You will need a GitHub PAT (Personal Access Token) from Codeberg for this.  
If you run the migration, the repository will be on Codeberg, but the authors are not linked.  
{{< figure src="/img/codeberg_overview_not_linked.png" caption="The author is not linked"  >}}
Also, if you have submitted your code originally with a GPG signature, you will see: "No known key found for this signature in database".  
{{< figure src="/img/codeberg_not_linked_no_gpg.png" caption="No known key found for this signature in database"  >}}
This will (in theory) work for most of the users as expected and maybe the commit is linked to the author correctly, but only if you have the same email address on GitHub and Codeberg.
As I try to [save the privacy of my real email address on GitHub](https://docs.github.com/en/account-and-profile/how-tos/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-on-github), I have a GitHub owned email address (e.g. `12345678+MYGITHUBUSERNAME@users.noreply.github.com`) , which I can't add to Codeberg, as I can never confirm the email address.
As I really wanted to see MY commits from the past, linked to my new Codeberg user, I had to run the extra mile here.

## Using my own script
So I used Microsoft Copilot to create a script to escape from it's own Microsoft / GitHub product:  
The end script is available on [Codeberg](https://codeberg.org/joergi/migrate_github_repo_to_codeberg)  
This script does a git history rewrite to link the authors to their Codeberg profiles.  
Normally, it's not recommended to change the git history, especially if others are also committing to this repository or have checked out the repository.  
But as this is just my own repository, it's fine.

if I start the migration with the script like:
```bash
./migrate_to_codeberg.sh https://github.com/joergi/rss-corners
```
or with: 
```bash
./migrate_to_codeberg.sh git@github.com:joergi/rss-corners.git
```

Here you can see how the history was rewritten:
{{< figure src="/img/codeberg_rewrite_history.png" caption="the history is rewritten"  >}}


So now it looks like I wanted it to look like:  
{{< figure src="/img/codeberg_overview_linked.png" caption="the commits are linked to the Codeberg author"  >}}

The original script I have migrated:
- GitHub -> https://github.com/joergi/rss-corners
- Codeberg -> https://codeberg.org/joergi/rss-corners

## ToDo:
- change the script to use more than one URL
- if a repo has more than one committer, make it possible to link them all to their Codeberg profiles
- check if there is a possibility to migrate the GitHub actions to [Codeberg/Forgejo actions](https://docs.codeberg.org/ci/actions/)
