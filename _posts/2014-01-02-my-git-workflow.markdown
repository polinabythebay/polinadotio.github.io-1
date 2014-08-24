---
layout: post
title:  "My Git Workflow"
date:   2014-01-02 19:55:40
categories: 
---
	
Version control is the bread and butter of open source and professional software development and yet I found it almost absent in my academic computer science education. Version control is a way to track changes to your software projects and manage collaboration. Git is the most widely adopted version control system for software development and anyone who's interested in coding should check it [out](http://gitimmersion.com/). 

For those completely new to it though, it can be really confusing to first download the Mac GUI client for [Github](https://mac.github.com/) and then realize it's pretty pointless for learning how to actually use and understand how a successful git branching model works. I will run through the basic commands you need to be successful with git and then describe how I do feature branching in my projects.

##Basic git

Assuming you've played around with git before, these are my usual go-to commands:

Configure git 

	$ git -v
	$ git config --global user.name "Your Name"
	$ git config --global user.email youremail@example.com
	$ git config --global user.username githubusername //configure for github


Committing changes to git

	$ git init . //only do this once to initialize git repository
	$ git add -A
	$ git commit -m "initial commit"


Creating a feature branch and then merging it into the main branch

	$ git checkout -b feature-branch 
	//creates and checks it out at the same time
	$ git add -A
	$ git commit -m "add feature changes"
	$ git checkout master
	$ git merge --squash feature-branch //squashes your commits
	$ git commit -m "add feature commits"
	$ git branch -D feature-branch //delete working branch

Checking in on a project

	$ git status //check status of changes
	$ git diff //view changes to files


Connecting a local reposity to an empty remote you just created on github and pushing changes to it

	$ git remote add origin <URLFROMGITHUB>
	$ git push origin master
	$ git remote add <REMOTENAME> <URL> 
	//add additional remote connections
	$ git pull <REMOTENAME> <BRANCHNAME> //pulling in changes
	$ git remote -v //view remote connections
	$ git push <REMOTENAME> <BRANCH> //push to remote connections


Forking and cloning projects

	$ git clone <URL> 
	//fork a project on github and then clone it to work on it locally


If the original repository you forked changes, you want to be able to pull in those changes, so you add a remote connection to the original. Most people name the remote connection 'upstream'.

	 $ git remote add upstream https://github.com/username/forkedproject.git

Add collaborators-- it makes software fun! You can add collaborators directly to your project via your github account. 

Working with collaborators

	$ git status 
	$ git fetch --dry-run //see changes to remote before you pull in
	$ git pull <REMOTENAME> <REMOTEBRANCH> 
	//pull in changes from remote branch

Creating a pull request 

* Visit original repository you forked on github,  http://www.github.com/username/projectname
* Click "Pull request" on the right-side menu, then "New pull request"
* Select branch with the changes you want to submit
* The page should then show the changes associated with your pull request. 

Merging a branch

	$ git checkout <BRANCHNAME>
	$ git merge <BRANCHNAME>
	$ git branch -D <BRANCHNAME>
	$ git push <REMOTENAME> -- delete <BRANCHNAME> 
	//you can also delete the branch from your fork on github

##Git rebase

Git rebase is a really awesome way to handle feature branch workflow. Some of the definitions of rebase are pretty convoluted but here's basically what you need to know about how git rebase works:

When you're working on a feature branch, rebasing the main development line into your feature branch will keep all of your feature branch commits at the head of the branch. - [Brad Robertson](http://infinitemonkeys.influitive.com/a-simple-explanation-for-git-rebase/)

Rebase works for your feature branches that aren't shared with your team. In other words, don't try rebasing into any shared branches (development, master, etc). If you do then everyone else will have to force pull your changes, and that's not helpful. 

Feature development workflow with git rebase:

* Create feature branch
* Develop as per usual
* Checkout main development branch by pulling/fetch

Rebase the dev branch into your feature branch by checking out the feature branch and running

	$ git rebase development

Push changes to remote. You might have to do a force push since you've changed the history on your feature branch

	$ git push -f
