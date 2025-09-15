---
title: Git
layout: default
parent: Tools
---

# Version control in COMPANY

Git (Global information tracker, or Goddamn idiotic truckload of s**t - when it breaks) is a tool to help developing software by recording changes in files and therefor crucial for the process of continuous integration.

A beginner friendly guide for Git can be found [here](https://www.git-help.com/).

## Prerequisites 
- A GitHub account & license.

## SSH

### Installation

Ensure the OpenSSH client is installed and available:

```
ssh -V
```
If it's not available install it via your package manager (e.g., apt, dnf, pacman).

### Generating keys
Generate the keys:
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Recommended algorithm: ed25519.

This creates:
  - Private key: id_ed25519
  - Public key: id_ed25519.pub

{: .warning}
The private key must remain secret; the public key is safe to share and upload to GitHub or other platforms.


### Adding keys to github and agent

Start ssh-agent and add your key:

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Copy your key into your clipboard:

```
xclip -sel clip < ~/.ssh/id_ed25519.pub
```
 
In GitHub go to:
- Settings
- SSH and GPG keys
- New SSH key 

Paste the public key.

Test the connection:
```
ssh -T git@github.com
```
{: .dos}
- Use a passphrase and the ssh-agent.
- Back up your private key securely.
- Consider separate keys for work and personal accounts.

{: .donts}
- Share or commit your private key.
- Use passwordless keys on shared devices.
- Email keys or paste them in chats.

## Git

### Installation
- Install Git via your OS package manager or the official installer.
- Verify:
```
git --version
```
- Basic global config:
```
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

### Usage


Clone a repository over SSH (find the SSH URL on GitHub: git@github.com:OWNER/REPO.git):
```
git clone git@github.com:OWNER/REPO.git
cd REPO
```
Create a branch and make changes:
```
git checkout -b feature/short-description
# edit files
git add .
git commit -m "feat: brief, imperative summary"
```
Sync with remote:
```
git pull --rebase
git push -u origin feature/short-description
```
View remotes and status:
```
git remote -v
git status
```

{: .note}
- Make small, focused commits with clear, imperative messages.
- Prefer present tense, concise subject line, optional body for context.
- Group related changes; avoid mixing refactors with behavior changes.
- Commit frequently during development; rebase/squash locally if needed before PR.
- Optionally follow Conventional Commits (feat, fix, docs, refactor, test, chore).

TODO: (chore:, wip:,  and so on)

### PR strategy

TODO