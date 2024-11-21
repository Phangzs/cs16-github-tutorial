# GitHub Workflow Guide

## Table of Contents
- [Introduction: Why GitHub?](#introduction-why-github)
- [The Git Workflow](#the-git-workflow)
  - [Commit](#commit)
  - [Push](#push)
- [Setting up GitHub SSH Keys on CSIL (or locally)](#setting-up-github-ssh-keys-on-csil-or-locally)
  - [What are SSH Keys?](#what-are-ssh-keys)
  - [Step 0: Pull up Relevant Windows (web browser and terminal/command prompt)](#step-0-pull-up-relevant-windows-web-browser-and-terminalcommand-prompt)
  - [Step 1: Log into GitHub with UCSB email](#step-1-log-into-github-with-ucsb-email)
  - [Step 2: Go to “Settings”](#step-2-go-to-settings)
  - [Step 3: Go to “SSH and GPG Keys” under “Access”](#step-3-go-to-ssh-and-gpg-keys-under-access)
  - [Step 4: Get SSH keys](#step-4-get-ssh-keys)
  - [Step 5: Input Keys and Test](#step-5-input-keys-and-test)
- [Example: GitHub Workflow](#example-calculator-app)
  - [Step 1: Clone the repository](#step-1-clone-the-repository)
  - [Step 2: Pull the latest changes](#step-2pull-the-latest-changes)
  - [Step 3: Check the repository status](#step-3-check-the-repository-status)
  - [Step 4: Remove an outdated file](#step-4-remove-an-outdated-file)
  - [Step 5: Add your name](#step-5-add-your-name)
  - [Step 6: Push the changes to GitHub](#step-6-push-the-changes-to-github)
- [Review: What did we do?](#review-what-did-we-do)
- [Learning More](#learning-more)

## Introduction: Why GitHub?
GitHub is a place to store files and save your progress. While you can store files on your computer and transport them yourself, by uploading your code to GitHub, you can view your current progress from any computer that can access that repository. In a lot of ways, GitHub is like Google Drive for code!

This makes GitHub supremely useful for working in teams and developing large pieces of software. In fact, as of 2024, over 90% of Fortune 100 companies use GitHub.

To interact with GitHub, we go through a “workflow.” A workflow typically means a sequence of steps to interact with a version control system, which in our case is GitHub. We will explore a workflow later. First, let’s define some terms.

## The Git workflow

### Commit
Suppose that you want to save your progress on your computer. You can commit those changes to do so. Committing is like hitting the save button. So you should try to commit as much as possible. Every commit is like a checkpoint—at any time, you can go back to a certain commit to “roll back” your code.

### Push
Now, suppose you want to save your progress to the internet so you can access it from somewhere else. You can push all of your commits to GitHub. Because the server is in the cloud (meaning you don’t know where the server is), you can consider your code to be permanently saved. Even if your laptop gets run over by a truck, an earthquake destroys all of UCSB, and a sudden sea level increase submerges all of Santa Barbara, you’ll still be able to access your code, if you can still make it to a laptop.

## Setting up GitHub SSH Keys on CSIL (or locally)

### What are SSH Keys?
SSH Keys are encrypted files that allow you to access remote systems and perform actions. For example, in order to log into CSIL, you set up some SSH keys to allow yourself to do that.

To perform GitHub actions from the terminal, we will generate and test another pair of SSH keys.

### Step 0: Pull up Relevant Windows (web browser and terminal/command prompt)
First, open a web browser and a terminal window. We will use the terminal to generate the keys and the web browser to link our local keys to GitHub.

### Step 1: Log into GitHub with UCSB email
Our first step is to log into the GitHub account that is linked to our UCSB emails.

### Step 2: Go to “Settings”
Next, go to the settings page by clicking on your profile picture on the top right and then clicking “Settings” near the bottom.

### Step 3: Go to “SSH and GPG Keys” under “Access”
On the left-hand bar, under access, click on SSH and GPG keys.

### Step 4: Get SSH keys

#### Step 4.0: Set path
We will first make sure we are in the correct directory. Issue the following command:
```bash
cd ~
```

#### Step 4.1: Create Key
Before we input any keys, we should generate them. We use ed25519 signing for better security (and the key is shorter). Replace the email with your UCSB email. The `-C` means comment, which is our string argument after it. We record our email for convenience.
```bash
ssh-keygen -t ed25519 -C "ucsb_net_id@ucsb.edu - github"
```

#### Step 4.2: Save key to file
Saving the key in the default location is more convenient, but if you already have a key in the default file path, then we would run into some issues. So we will use a custom path.

Type `.ssh/github` so that you see the following text:
```
> Enter file in which to save the key (/cs/student/[username]/.ssh/id_ed25519): .ssh/github
```

#### Step 4.3: (Don’t) Create a passphrase
For extra security, you may save a passphrase for this key. However, doing so would require the passphrase every time the key is invoked, so any pulls and pushes would require a passphrase. Therefore, I recommend not using a passphrase.

The passphrase prompt will repeat for security purposes.
```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

#### Step 4.4: Add the private key to the ssh-agent
The private keys go to the local machine. We first must start the ssh-agent in the background.
```bash
eval "$(ssh-agent -s)"
```

We then add our private key to the ssh-agent.
```bash
ssh-add ~/.ssh/github
```

#### Step 4.5: Copy the public key
We need to copy the public key in order to put it into GitHub. Issue the following command:
```bash
cat ~/.ssh/github.pub
```

Copy everything that follows (it should be short).

### Step 5: Input Keys and Test
In the browser, we will now click the green button that says “New SSH Key.” We can give this key a name to keep track of it in GitHub. Issue the following command to test our keys.
```bash
ssh -T git@github.com
```

Let’s try an example workflow with a calculator program.

### Example: Adding your name
We will use an example repository for you to practice a basic Git workflow.

#### Step 1: Clone the repository
To start, you need to get a copy of the repository on your local machine. You can get the repository URL from the repository GitHub page - look for the “Code” button that looks like this:

Click on it and copy the information under the “SSH” tab.

Then use the following command in your terminal:
```bash
git clone <repository_url>
```
`<repository_url>` will be replaced with the example repository url.

#### Step 2: Pull the latest changes
Before making any modifications, ensure you have the latest version of the code. Run:
```bash
git pull
```
Note: This is not necessary in this case because we cloned the repository, but it’s a good idea to pull in general.

#### Step 3: Check the repository status
Let’s see what files are currently being tracked and if there are any changes:
```bash
git status
```
This will show a list of modified, untracked, or deleted files.

#### Step 4: Remove an outdated file
Suppose a file in `random_files` is no longer relevant. We’ll remove it:
```bash
rm random_files/[num].txt
```
Replace `[num]` with any number from 0-1000 (that still exists in the folder). Now, tell Git that we want to stage this deletion:
```bash
git rm random_files/[num].txt
```
Finally, commit the removal with a descriptive message:
```bash
git commit -m "Remove random file [num]"
```

#### Step 5: Add your name
Open the `names.txt` file with Vim.
```bash
vim names.txt
```
Go to the last line by pressing `G` in normal mode. Then add your name to the bottom of the list.

**Original List (Example):**
```
Please write down your name or your username. Add it to the bottom of the list on a new line.
DO NOT USE YOUR FULL NAME. 
Phangzs
```

**New List (Example):**
```
Please write down your name or your username. Add it to the bottom of the list on a new line.
DO NOT USE YOUR FULL NAME. 
Phangzs
[your name or username]
```
Save the changes. Stage the updated file:
First, exit the file. Then issue the following command:
```bash
git add names.txt
```
Commit the changes with a meaningful message:
```bash
git commit -m "added name"
```

#### Step 6: Push the changes to GitHub
Now that we’ve made several changes, let’s push them to the remote repository:
```bash
git push
```

You may find that you get an error message:

```bash
To github.com:Phangzs/cs16-github-tutorial.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:Phangzs/cs16-github-tutorial.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This means that somebody else has updated the repository after you last did git pull. To fix this, we need to merge the current repository with our local files.

```bash
git merge
```

A nano shell will pop up. Just save with control-x and type y in the prompt (for yes). After successfully saving, we will push again.

```bash
git push
```


## Review: What did we do?
1.	Pulled the latest changes to ensure we were working with the most up-to-date code.
2.	Removed an random file (`[num].txt`) and staged the removal.
3.	Added your (user)name in `names.txt`.
4.	Pushed all changes to this repository to make them available to everyone.

## Learning More
Now that you’ve gone through a simple Git workflow, try experimenting with other commands like branching, merging, and resolving conflicts. Git is a powerful tool that can help you collaborate effectively and keep your codebase organized.

You can use the Git guide for reference:
```
git --help
```
