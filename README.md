# GitHub Tutorial

## Table of Contents
- [Introduction: Why GitHub?](#introduction-why-github)
- [The Git Workflow](#the-git-workflow)
  - [Commit](#commit)
  - [Push](#push)
- [Setting up GitHub SSH Keys](#setting-up-github-ssh-keys-on-csil-or-locally)
  - [What are SSH Keys?](#what-are-ssh-keys)
  - [Step 0: Pull up Relevant Windows (web browser and terminal/command prompt)](#step-0-pull-up-relevant-windows-web-browser-and-terminalcommand-prompt)
  - [Step 1: Log into GitHub with UCSB email](#step-1-log-into-github-with-ucsb-email)
  - [Step 2: Go to "Settings"](#step-2-go-to-settings)
  - [Step 3: Go to "SSH and GPG Keys" under "Access"](#step-3-go-to-ssh-and-gpg-keys-under-access)
  - [Step 4: Get SSH keys](#step-4-get-ssh-keys)
  - [Step 5: Input keys and test](#step-5-input-keys-and-test)
- [Example: Git Workflow with a Repository](#example-git-workflow-with-a-repository)
  - [Clone](#clone)
  - [Pull](#pull)
  - [Status](#status)
  - [Add](#add-a-log-file)
  - [Commit Messages](#commit-messages)
  - [Push (origin main)](#push-the-changes-to-github)
- [Review: What did we do?](#review-what-did-we-do)
- [Learning More](#learning-more)

## Introduction: Why GitHub?
GitHub is a place to store files and to save your progress. While you can store files on your computer and transport them yourself, by uploading your code to GitHub, you can see your current progress from any computer that can access that repository. In a lot of ways, GitHub is like Google Drive for code!

This makes GitHub supremely useful for working in teams and developing large pieces of software. In fact, as of 2024, over [90%](https://www.google.com/url?q=https://github.blog/engineering/engineering-principles/githubs-engineering-fundamentals-program-how-we-deliver-on-availability-security-and-accessibility/&sa=D&source=docs&ust=1732237174877627&usg=AOvVaw3yCvXr4sCOH7jzdJSvaxJB) of Fortune 100 companies use GitHub.

To interact with GitHub, we go through a “workflow.” A workflow typically means a sequence of steps to interact with a version control system, which in our case is GitHub. We will explore a workflow later. First, let’s define some terms.

## The Git Workflow
### Commit
Suppose that you want to save your progress on your computer. You can commit those changes to do so. Committing is like hitting the save button. So you should try to commit as much as possible. Every commit is like a checkpoint—at any time, you can go back to a certain commit to “roll back” your code.

### Push
Now, suppose you want to save your progress to the internet so you can access it from somewhere else. You can push all of your commits to GitHub. Because the server is in the cloud (meaning you don’t know where the server is), you can consider your code to be permanently saved. Even if your laptop gets run over by a truck, an earthquake destroys all of UCSB, and a sudden sea level increase submerges all of Santa Barbara, you’ll still be able to access your code, if you can still make it to a laptop.

## Setting up GitHub SSH Keys on CSIL (or locally)
### What are SSH Keys?
SSH Keys are encrypted files that allow you to access remote systems and perform actions. For example, in order to log into CSIL, you set up some SSH keys to allow yourself to do that.

To perform GitHub actions from the terminal, we will generate and test another pair of SSH keys.

#### Step 0: Pull up Relevant Windows (web browser and terminal/command prompt)
First open a web browser and a terminal window. We will use the terminal to generate the keys and the web browser to link our local keys to GitHub.

#### Step 1: Log into GitHub with UCSB email
Our first step is to log into the GitHub account that is linked to our UCSB emails.

#### Step 2: Go to “Settings”
Next, go to the settings page by clicking on your profile picture on the top right and then clicking “Settings” near the bottom.

#### Step 3: Go to “SSH and GPG Keys” under “Access”
On the left-hand bar, under access, click on SSH and GPG keys.

#### Step 4: Get SSH keys
- **Step 4.0: Set path**  
  We will first make sure we are in the correct directory. Issue the following command:  
  ```bash
  cd ~
  ```

- **Step 4.1: Create Key**  
  Before we input any keys, we should generate them. We use ed25519 signing for better security (and the key is shorter). Replace the email with your ucsb email. The `-C` means comment, which is our string argument after it. We record our email for convenience.
  ```bash
  ssh-keygen -t ed25519 -C "ucsb_net_id@ucsb.edu - github"
  ```

- **Step 4.2: Save key to file**  
  Saving the key in the default location is more convenient, but if you already have a key in the default file file path, then we would run into some issues. So we will use a custom path. Type `.ssh/github` so that you see the following text:  
  ```plaintext
  > Enter file in which to save the key (/cs/student/[username]/.ssh/id_ed25519): .ssh/github
  ```

- **Step 4.3: (Don’t) Create a passphrase**  
  For extra security, you may save a passphrase for this key. However, doing so would require the passphrase every time the key is invoked, so any pulls and pushes would require a passphrase. Therefore, I recommend not using a passphrase.  
  The passphrase prompt will repeat for security purposes.
  ```plaintext
  > Enter passphrase (empty for no passphrase): [Type a passphrase]  
  > Enter same passphrase again: [Type passphrase again]
  ```

- **Step 4.4: Add the private key key to the ssh-agent**  
  The private keys goes to the local machine. We first must start the ssh-agent in the background.
  ```bash
  eval "$(ssh-agent -s)"
  ```
  We then add our private key to the ssh-agent.
  ```bash
  ssh-add ~/.ssh/github
  ```

- **Step 4.5: Copy the public key**  
  We need to copy the public key in order to put it into GitHub. Issue the following command:
  ```bash
  cat ~/.ssh/github.pub
  ```
  Copy everything that follows (it should be short).

#### Step 5: Input keys and test 
In the browser, we will now click the green button that says “New SSH Key”. We can give this key a name to keep track of it in GitHub. Issue following command to test our keys.
```bash
ssh -T git@github.com
```

## Example: Git Workflow with a Repository
Let’s try an example workflow with a calculator program. We will use an example repository for you to practice a basic Git workflow.

### Clone
**Step 1: Clone the repository**  
To start, you need to get a copy of the repository on your local machine. You can get the repository URL from the repository GitHub page - look for the “Code” button. Click on it and copy the information under the “SSH” tab. Then use the following command in your terminal:
```bash
git clone git@github.com:ucsb-cs16-f24/github-tutorial.git
```

### Pull
**Step 2: Pull the latest changes**  
Before making any modifications, ensure you have the latest version of the code. Run:
```bash
git pull
```
Note: This is not necessary in this case we cloned the repository, but it’s a good idea to pull in general.

### Status
**Step 3: Check the repository status**  
Let’s see what files are currently being tracked and if there are any changes:
```bash
git status
```
This will show a list of modified, untracked, or deleted files.

### Add a Log File
**Step 4: Add a log file**  
Open a new log file, `logs.txt`, with Vim.
```bash
vim logs.txt
```
Type in any sort of log file. For example:  
`Nov 21: Erik creates this log file.`

Press `esc` followed by a colon `(:)` and `wq` to exit and save the file. Now, we need to stage this creation.
```bash
git add logs.txt
```

### Commit Messages
**Step 5: Fix the bug in `calculator.cpp`**

- **Step 5.1: Test the error**  
  Currently, the calculator has a minor bug! We can verify this by compiling and running it.
  ```bash
  make; ./calculator
  ```
  Now if we try to divide 0 by 0, we get `-nan`.

  ```plaintext
  Simple Calculator  
  Enter an operation (+, -, *, /): /  
  Enter the first number: 0  
  Enter the second number: 0  
  Result: -nan  
  ```

- **Step 5.2: Change the code**  
  Let’s change the output when we divide by 0. Change the calculator logic for the division case so that a division by 0 results in an output of `Error: Division by 0!` to the standard error stream (via `cerr`), and terminates the program with an error code of 1.

- **Step 5.3: Test the corrected code**  
  By removing the old executable, compiling a new one and running it to test our edge case, we can confirm that we’ve fixed the error.
  ```bash
  rm calculator; make; ./calculator
  ```
  Now if we try to divide 0 by 0, we get an error message.
  ```plaintext
  Simple Calculator  
  Enter an operation (+, -, *, /): /  
  Enter the first number: 0  
  Enter the second number: 0  
  Error: Division by 0!  
  ```

  This confirms that we have covered our test case, and concludes our bug fixing.

- **Step 5.4: Stage the changes**  
  We can now stage the changes we made.
  ```bash
  git add calculator.cpp
  ```

**Step 6: Remove an outdated file**  
The error text file is irrelevant now that we have fixed the error. You may take a look at the file.
```bash
cat errors.txt
```
However, we’re going to remove the file now that we have fixed the issue.
```bash
rm errors.txt
```
We also need to stage this change.
```bash
git rm errors.txt
```

**Step 7: Commit all changes**  
Now that we have staged all of the changes, we have completed our task. It’s time to commit all of our changes and save our progress.
```bash
git commit -m "Fixed divide by 0 errors."
```

### Push the Changes to GitHub
**Step 8: Push the changes to GitHub**  
Now that we’ve made several changes, let’s push them to the remote repository:
```bash
git push
```

## Review: What did we do?
1. Pulled the latest changes to ensure we worked with the most up-to-date code.
2. Added a log file in `logs.txt` and staged the addition.
3. Fixed a bug in `calculator.cpp` and staged the addition.
4. Removed an errors file and staged the removal.
5. Committed all changes.
6. Pushed all changes to the remote repository to make them available to everyone.

## Learning More
Now that you’ve gone through a simple Git workflow, try experimenting with other commands like branching, merging, and resolving conflicts. Git is a powerful tool that can help you collaborate effectively and keep your codebase organized.

You can use the Git guide for reference:
```bash
git -g
```


