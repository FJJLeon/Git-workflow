# Git workflow

## 1.What is Git workflow
As a source management system, Git inevitably involves multi-person cooperation. Then when we work together with others, there must be a standardized workflow which helps us cooperate effectively and make the project develop in good order. Yes, the workflow is Git workflow. 

A Git Workflow is a recipe or recommendation for how to use Git to accomplish work in a consistent and productive manner. Git workflows encourage users to leverage Git effectively and consistently. Git offers a lot of flexibility in how users manage changes. Given Git's focus on flexibility, there is no standardized process on how to interact with Git. When working with a team on a Git managed project, it’s important to make sure the team is all in agreement on how the flow of changes will be applied. To ensure the team is on the same page, an agreed upon Git workflow should be developed or selected. 

## 2.Why we need Git workflow
When it comes to version control system, **svn** comes into someone's mind. SVN is a centralized version control system in which each developer gets a working copy that points back to a single central repository. We cannot use this thought to use Git. **Git** is a distributed version control system. Instead of a working copy, each developer gets their own local repository, complete with a full history of commits. There are a team of developer changing the code at a time, so you can not change the main code. Instead you should create a branch and make change here. When you finish your change, you should add it to the main code if allowed.

## 3.How we use Git workflow
There are some kinds of different Git workflow such as **Centralized Workflow**,**Feature Branch Workflow**, **Gitflow Workflow**, **Forking Workflow**. However, I think it is not which Git workflow you choose but whethe the Git workflow chosen suit your need and your team that matters.

###Take Centralized Workflow as an example

### Step1. Initialize the central repository
First, someone needs to create the central repository on a server. If it’s a new project, you can initialize an empty repository. Otherwise, you’ll need to import an existing Git or SVN repository.

Central repositories should always be bare repositories (they shouldn’t have a working directory), which can be created as follows:

> ssh user@host git init --bare /path/to/repo.git

Be sure to use a valid SSH username for 'user', the domain or IP address of your server for 'host', and the location where you'd like to store your repo for '/path/to/repo.git'. Note that the '.git 'extension is conventionally appended to the repository name to indicate that it’s a bare repository.

### Step2. Hosted central repositories
Central repositories are often created through 3rd party Git hosting services like Bitbucket Cloud or Bitbucket Server. The process of initializing a bare repository discussed above is handled for you by the hosting service. The hosting service will then provide an address for the central repository to access from your local repository.

### Step3. Clone the central repository
Each developer creates a local copy of the entire project.This is accomplished via the 'git clone' command:

> git clone ssh://user@host/path/to/repo.git

When you clone a repository, Git automatically adds a shortcut called `origin` that points back to the “parent” repository, under the assumption that you'll want to interact with it further on down the road. 

### Step4. Make changes and commit
Once the repository is cloned locally, a developer can make changes using the standard Git commit process: edit, stage, and commit. If you’re not familiar with the staging area, it’s a way to prepare a commit without having to include every change in the working directory. This lets you create highly focused commits, even if you’ve made a lot of local changes.

> git status # View the state of the repo  
git add <some-file> # Stage a file  
git commit # Commit a file</some-file>

Remember that since these commands create local commits, John can repeat this process as many times as he wants without worrying about what’s going on in the central repository. This can be very useful for large features that need to be broken down into simpler, more atomic chunks.

### Step5. Push new commits to central repository
Once the local repository has new changes committed. These change will need to be pushed to share with other developers on the project.

> git push origin master

This command will push the new committed changes to the central repository. When pushing changes to the central repository, it is possible that updates from another developer have been previously pushed that contain code which conflict with the intended push updates. Git will output a message indicating this conflict. In this situation, `git pull` will first need to be executed. This conflict scenario will be expanded on in the following section.

### Step6. Managing conflicts
The central repository represents the official project, so its commit history should be treated as sacred and immutable. If a developer’s local commits diverge from the central repository, Git will refuse to push their changes because this would overwrite official commits.

Before the developer can publish their feature, they need to fetch the updated central commits and rebase their changes on top of them. This is like saying, “I want to add my changes to what everyone else has already done.” The result is a perfectly linear history, just like in traditional SVN workflows.

If local changes directly conflict with upstream commits, Git will pause the rebasing process and give you a chance to manually resolve the conflicts. The nice thing about Git is that it uses the same `git status` and `git add` commands for both generating commits and resolving merge conflicts. This makes it easy for new developers to manage their own merges. Plus, if they get themselves into trouble, Git makes it very easy to abort the entire rebase and try again (or go find help).


#### Reference:[Git Workflows and Tutorials](https://www.atlassian.com/git/tutorials/)
