---
title: Getting Up in the GitHub
layout: post
---

*How do I upload a project on my computer to GitHub?*

Seems like an easy question, but if you've never done it before, it might be a
bit troublesome at first. And while the [GitHub](https://www.github.com)
[Help Documentation](https://help.github.com/) is tight, and the
[Bootcamp](https://help.github.com/categories/54/articles) articles are super
helpful, sometimes people just want a simple walkthrough that explains how to
move a project or website they have on their computer to a repository on
GitHub. 

If you are one of these "people", then hopefully this short article will help
you get all up in GitHub. The process is pretty straightforward, and after you
do it once, the next time will be easy. Before we start, we'll just assume
you've setup a 
[command line git client](https://help.github.com/articles/set-up-git) and that
you've already [signed up](https://github.com/) for a GitHub account.

### Pushing That Project to GitHub

So let's say we have a project on our computer, some next level website for
example:

    $ ls ~/work/next-level/
    README.md index.html

The first thing we need to do is [initialize](http://gitref.org/creating/#init)
a git repository in our project directory:

    $ cd ~/work/next-level
    $ git init
    Initialized empty Git repository in /Users/robert/work/next-level/.git/

Then we need to [add](http://gitref.org/basic/#add) our files to be tracked:

    $ git add .

And then we can make an initial [commit](http://gitref.org/basic/#commit) of
all our project files:

    $ git commit -m "Initial commit."
    [master (root-commit) d7b316c] Initial commit.
     2 files changed, 35 insertions(+)
     create mode 100644 README.md
     create mode 100644 index.html

So now we have a local git repository for our project, and we're ready to push
our files to GitHub. First, we need to
[create a repository](https://github.com/new), and enter a **Repository
name** and optionally a **Description** on the new repository page. But don't
select the checkbox for **Initialize this repository with a README** since we
want an empty repository because we're pushing our existing files:

![](http://f.cl.ly/items/0G2Z133A333k1Y3D0y0y/new-repository.png)

Then back on our computer, we want to create a
[remote](http://gitref.org/remotes/#remote) that points to our new GitHub
repository. Since we'll be pushing to this repository, we'll want to use
either the
[SSH URL or the HTTPS URL](https://help.github.com/articles/which-remote-url-should-i-use).
These URLs will always be of the form:

    # SSH URL
    git@github.com:<username>/<repository-name>.git

    # HTTPS URL
    https://github.com/<username>/<repository-name>.git

The new repository page also gives us the URLs we can use to work with our 
repository, and further down, the page even lists the remaining steps we'll
need to run to push our local repository to GitHub (which we'll get to in a
second):

![](http://f.cl.ly/items/180e471f3Z0e193N0C34/new-repository-created.png)

Now that we know the repository URLs, we can choose one to add a remote that
points to our GitHub repository. We'll use the SSH URL in this example (you'll need to have
[setup your SSH Keys](https://help.github.com/articles/generating-ssh-keys) to
use this URL):

    $ git remote add origin git@github.com:rsese/next-level.git

And finally, we can [push](http://gitref.org/remotes/#push) our project up to GitHub:

    $ git push -u origin master
    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 865 bytes, done.
    Total 4 (delta 0), reused 0 (delta 0)
    To git@github.com:rsese/next-level.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.

Refresh that repository page, and we should see our files now:

![](http://f.cl.ly/items/1B0l1A3w290g2U0P3O1k/files-uploaded-pushed.png)

And that's it, we're done, time for profit.

### The End.

Hopefully this article was easy to follow, and getting projects from your
computer to a repository on GitHub is super clear. Also, if you happen to be new to git and GitHub and want to do more reading, check out the references
section below to have knowledge dropped on you.

### RTFM and Other Resources

* Git Reference. [http://gitref.org/](https://help.github.com/).  
A reference to the most commonly used git commands.
* Pro Git. [http://git-scm.com/book](https://help.github.com/).  
A great reference overall, but
[chapter 1]([http://git-scm.com/book/en/Getting-Started](https://help.github.com/)
and
[chapter 2]([http://git-scm.com/book/en/Git-Basics](https://help.github.com/)
specifically are good beginning material.
* github:help. [https://help.github.com](https://help.github.com/).  
GitHub's official help articles. Covers most of the the tasks you'll need to
perform on GitHub.
