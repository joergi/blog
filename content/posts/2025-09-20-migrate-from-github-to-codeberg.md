---
title: "Migrating from GitHub to Codeberg"
description: "Using if you use the official Codeberg migration tool the authors are not linked. so this script does a git history rewrite to link the authors to their Codeberg profiles."
date: 2025-09-20
---

## Reasons for migration
There are many reasons, why many of the open source projects are migrating from GitHub to Codeberg.
GitHub Copilot, the AI from GiHub which is owned by Microsoft, gives us the following reasons:
```text
Main reason: many projects prefer a community‑run, non‑profit, open infrastructure (Codeberg e.V. running Forgejo/Gitea) over GitHub’s proprietary, Microsoft‑controlled platform with increasing commercialization (e.g. AI features, data usage, lock‑in concerns). Privacy, transparency, open governance, and avoiding dependence on a single corporate vendor are the core motivators.
``` 
When then asking GitHub Copilot for political reasons, it gave me the following list:
```text
1. U.S. jurisdiction and exposure to sanctions or export controls (accounts in sanctioned regions restricted).
2. Past controversy over Microsoft (GitHub owner) contracts with U.S. government agencies (e.g. immigration enforcement) raising ethical concerns for some contributors.
3. DMCA takedown handling and perceived overreach (e.g. high‑profile repository removals) creating fears of centralized control.
4. Risk of sudden access loss for developers in geopolitically sensitive regions.
5. Concern over corporate influence on open tooling versus community governance (Codeberg is a German non profit).
6. Desire for stronger EU privacy and data protection alignment (GDPR emphasis).
7. Objections to use of public code for large scale AI training without explicit opt in.
8. Preference to reduce single vendor dependency and potential policy shifts tied to U.S. politics.
```
So, yeah, there are a lot of reasons to migrate.  
So I wanted to migrate some of my repos too and I had a look how this could be done.

## Official migration tool from Codeberg
If you go to Codeberg, there is an official migration tool:
{{< figure src="img/codeberg_migration_button.png" caption="Button for doing the migration"  >}}
You will need a GitHub PAT (Personal Access Token) from Codeberg for this.  
If you run the migration, the repository will be on Codeberg, but the authors are not linked.  
{{< figure src="img/codeberg_overview_not_linked.png" caption="The author is not linked"  >}}
Also, if you have submitted your code originally with a GPG signature, you will see: "No known key found for this signature in database".  
{{< figure src="img/codeberg_not_linked_no_gpg.png" caption="No known key found for this signature in database"  >}}
This will (in theory) work for most of the users as expected and maybe the commit is linked to the author correctly, but only if you have the same email address on GitHub and Codeberg.
As I try to [save the privacy of my real email address on GitHub](https://docs.github.com/en/account-and-profile/how-tos/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-on-github), I have a GitHub owned email address (e.g. 12345678+MYGITHUBUSERNAME@users.noreply.github.com) , which I can't add to Codeberg, as I can never confirm the email address.
As I really wanted to see MY commits from the past, linked to my new Codeberg user, I had to run the extra mile here.
{{< figure src="img/codeberg_overview_linked.png" caption="the commits are linked to the Codeberg author"  >}}

## Using my own script
So I used Microsoft Copilot to create a script to escape from it's own Microsoft / GitHub product:  
The end script is available on [Codeberg](https://codeberg.org/joergi/migrate_github_repo_to_codeberg)
This script does a git history rewrite to link the authors to their Codeberg profiles.
Normally, it's not recommended to change the git history, especially if others are also committing to this repository or have checked out the repository.


## ToDo:
- change the script to use more than one URL
- if a repo has more than one committer, make it possible to link them all to their Codeberg profiles