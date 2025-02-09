# Multiple GitHub Accounts on a Single Device
Instructions on how to use multiple GitHub accounts on a single device.
This example will use accounts called "personal" and "work".


### Create your SSH keys if you haven't already
```bash
ssh-keygen -t ed25519 -C "personal_email@example.com" -f $env:USERPROFILE\.ssh\id_ed25519
ssh-keygen -t ed25519 -C "work_email@example.com" -f $env:USERPROFILE\.ssh\id_ed25519_work
```


### Start your SSH agent and add your keys
```bash
# Start the service
Start-Service ssh-agent
# Add both keys
ssh-add $env:USERPROFILE\.ssh\id_ed25519
ssh-add $env:USERPROFILE\.ssh\id_ed25519_work
# Confirm the keys have been added
ssh-add -l
```

### Add your keys to GitHub. Repeat for each account.
**Account 1 (Personal)**
```bash
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```
Login to your **personal** GitHub account > Settings > SSH and GPG Keys > New SSH Key and paste the key into your account.

**Account 2 (Work)**
```bash
Get-Content $env:USERPROFILE\.ssh\id_ed25519_work.pub
```
Login to your **work** GitHub account > Settings > SSH and GPG Keys > New SSH Key and paste the key into your account.


### Create an SSH config
```bash
notepad $env:USERPROFILE\.ssh\config
```
Paste this into your config file and save:
```bash
# GitHub Account 1 (Personal)
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519

# GitHub Account 2 (Work)
Host work.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
```
Find the file in your explorer and delete the '.txt' file type. Press ok to any warnings.


### Set your repo's git credentials
When working in a repository on your machine that coincides with your **personal** account set the username and email that coincides with your personal GitHub account. And vice versa for your **work** GitHub account.
```bash
git config user.name "Your Name"
git config user.email "your_email_account1@example.com"
```


### Test your SSH connection
```bash
ssh -T git@github.com         # personal
ssh -T git@work.github.com    # work
```


### To clone a respository
```bash
# Personal
git clone git@github.com:username/repository.git
# Work
git clone git@work.github.com:username/repository.git
```


### Reset existing repositories
```bash
# Personal
git remote set-url origin git@github.com:username/repository.git
# Work
git remote set-url origin git@work.github.com:username/repository.git
# Verify the change worked using:
git remote -v
```
