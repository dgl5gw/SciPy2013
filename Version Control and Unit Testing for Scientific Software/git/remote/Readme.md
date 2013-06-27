# Remote Version Control

## github.com?

GitHub is a site where many people store their open (and closed) source
code repositories. It provides tools for browsing, collaborating on and
documenting code. Your home institution may have a repository hosting
system of its own. To find out, ask your system administrator.  GitHub,
much like other forge hosting services (
[launchpad](https://launchpad.net), [bitbucket](https://bitbucket.org),
[googlecode](http://code.google.com), [sourceforge](http://sourceforge.net)
etc.) provides :

-   landing page support 
-   wiki support
-   network graphs and time histories of commits
-   code browser with syntax highlighting
-   issue (ticket) tracking
-   user downloads
-   varying permissions for various groups of users
-   commit triggered mailing lists
-   other service hooks (twitter, etc.)

## github password 

Setting up github requires a github user name and password.  Please take a
moment to [create a free github account](https://github.com/signup/free) (if you
want to start paying, you can add that to your account some other day).

## git remote : Steps for Forking a Repository

A key step to interacting with an online repository that you have forked
is adding the original as a remote repository. By adding the remote
repository, you inform git of a new option for fetching updates and
pushing commits.

The **git remote** command allows you to add, name, rename, list, and
delete repositories such as the original one **upstream** from your
fork, others that may be **parallel** to your fork, and so on.

### Exercise : Fork Our GitHub Repository

While you may already have a copy of this repository, GitHub doesn't know about 
it until you've made a fork. You'll need to tell github you want to have an 
official fork of this repository.

Step 1 : Go to our
[repository](https://github.com/jiffyclub/scipy-2013-git-testing)
from your browser, and click on the Fork button. Choose to fork it to your
username rather than any organizations.

Step 2 : Clone it. From your terminal :

    $ git clone https://github.com/YOU/scipy-2013-git-testing.git
    $ cd scipy-2013-git-testing

Step 3 : 

    $ git remote add upstream https://github.com/jiffyclub/scipy-2013-git-testing.git
    $ git remote -v
    origin          https://github.com/YOU/scipy-2013-git-testing.git (fetch)
    origin          https://github.com/YOU/scipy-2013-git-testing.git (push)
    upstream        https://github.com/jiffyclub/scipy-2013-git-testing.git (fetch)
    upstream        https://github.com/jiffyclub/scipy-2013-git-testing.git (push)

All repositories that are clones begin with a remote called origin.

## git fetch : Fetching the contents of a remote

Now that you have alerted your repository to the presence of others, it
is able to pull in updates from those repositories. In this case, if you
want your master branch to track updates in the original SWC-bootcamp
repository, you simply **git fetch** that repository into the master
branch of your current repository.

The fetch command alone merely pulls down information recent changes
from the original master (upstream) repository. By itself, the fetch
command does not change your local working copy. To update your local
working copy to include recent changes in the original (upstream)
repository, it is necessary to also merge.

## git merge : Merging the contents of a remote

Now, your repository looks just like ours. However, we might change something to 
our repository, and you'll want to keep up to date. 

To incorporate upstream changes from the original master repository (in
this case jiffyclub/scipy-2013-git-testing) into your local working copy, you
must both fetch and merge. The process of merging may result in
conflicts, so pay attention. This is where version control is both at
its most powerful and its most complicated.

### Exercise : Fetch and Merge the Contents of Our GitHub Repository

Step 1 : Fetch the recent remote repository history

    $ git fetch upstream

Step 2 : Merge the upstream master branch into your master branch

    $ git merge upstream/master

Step 3 : Find out what happened by browsing the directory. 

## git pull : Pull = Fetch + Merge

The command **git pull** is the same as executing **git fetch** followed
by **git merge**. Though it is not recommened for cases in which there
are many branches to consider, the pull command is shorter and simpler
than fetching and merging as it automates the branch matching.
Specificially, to perform the same task as we did in the previous
exercise, the pull command would be :

    $ git pull upstream master
    Already up-to-date.

When there have been remote changes, the pull will apply those changes to your 
local branch. It may require conflict resolution if there are conflicts with 
your local changes.

## git push : Sending Your Commits to Remote Repositories

The **git push** command pushes commits in a local working copy to a remote 
repository. The syntax is git push [remote] [local branch]. To push to a 
differently named remote branch, the syntax is git push [local branch]:[remote 
branch].  Before pushing, a developer should always pull (or fetch + merge), so 
that there is an opportunity to resolve conflicts before pushing to the remote.

### Exercise : Push a change to github
We'll talk about conflicts later, but first, since we have no conflicts
and are up to date, we can make a minor change and send our changes to
your fork, the "origin."

    $ git push origin master

If you have permission to push to the upstream repository, sending
commits to that remote is exactly analagous.

    $ git push upstream master

In the case of the master code, new developer accounts will not allow
this push to succeed. You're welcome to try it though.

## git merge : Conflicts

This is the trickiest part of version control, so let's take it very
carefully.

In the master code, you'll find a file called Readme.md. This is a standard 
documentation file that appears rendered on the landing page for the repository 
in github. To see the rendered version, visit your fork on github, 
(https://github.com/YOU/scipy-2013-git-testing/tree/master/README.md).

For illustration, let's imagine that, suddenly, each of the developers
on the master code would like to welcome visitors in a language other
than English. Since we're all from so many different places and speak
so many languages, there will certainly be disagreements about what to
say instead of "Welcome."

I, for example, am from Texas, so I'll push (to the upstream
repository) my own version of Welcome on line 5 of Readme.md.

You may speak another language, however, and may want
to replace Welcome with an equivalent word that you
prefer (willkommen, bienvenido, benvenuti, vanakkam, etc.).

You'll want to start a new branch for development. It's a good convention
to think of your master branch (in this case your master branch) as
the "production branch," typically by keeping that branch clean of your
local edits until they are ready for release. Developers typically use the
master branch of their local fork to track other developers changes in the
remote repository until their own local development branch changes are
ready for production.

### Exercise : Experience a Conflict *

Step 1 : Make a new branch 

Step 2 : Edit the readme file in that branch,

Step 3 : and commit your changes.

Step 4 : Mirror the remote upstream repository in your master branch 
by pulling down my changes

Step 5 : You want to push it to the internet eventually, so you merge updates 
from development, but may experience a conflict.


## git resolve : Resolving Conflicts

Now what?

Git has paused the merge. You can see this with the **git status**
command.

    # On branch master
    # Unmerged paths:
    #   (use "git add/rm <file>..." as appropriate to mark resolution)
    #
    #       unmerged:      Readme.md
    #
    no changes added to commit (use "git add" and/or "git commit -a")

The only thing that has changed is the Readme.md file. Opening it,
you'll see something like this at the beginning of the file.

    =====================
    <<<<<<< HEAD
    Howdy
    =======
    Willkommen
    >>>>>>> development
    =====================

The intent is for you to edit the file, knowing now that I wanted the
Welcome to say Howdy. If you want it to say Willkommen, you should
delete the other lines. However, if you want to be inclusive, you may
want to change it to read Vanakkam and Willkommen. Decisions such as this
one must be made by a human, and why conflict resolution is not handled
more automatically by the version control system.

    Howdy and Willkommen

This results in a status 

what status
    # On branch master
    # Unmerged paths:
    #   (use "git add/rm <file>..." as appropriate to mark resolution)
    #
    # both modified:      readme.rst
    #
    ...
    no changes added to commit (use "git add" and/or "git commit -a")


To alert git that you have made appropriate alterations, follow the instructions 
it gave you in the status message :

    $ git add Readme.md
    $ git commit
    Merge branch 'development'

    Conflicts:
      Readme.md
    #
    # It looks like you may be committing a MERGE.
    # If this is not correct, please remove the file
    # .git/MERGE_HEAD
    # and try again.
    #
    $ git push origin master
    Counting objects: 10, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (6/6), 762 bytes, done.
    Total 6 (delta 2), reused 0 (delta 0)
    To git@github.com:username/scipy-2013-git-testing.git


## * Solution to the conflict exercise

    $ git branch development
    $ git checkout development
    Switched to branch 'development'
    $ nano Readme.md 
    <edit the readme file and exit the text editor>
    $ git commit -am "Changed the welcome message to ... "
    $ git checkout master
    Switched to branch 'master'
    $ git fetch upstream
    $ git merge upstream/master
    $ git merge development
    Auto-merging Readme.md
    CONFLICT (content): Merge conflict in Readme.md
    Automatic merge failed; fix conflicts and then commit the result.
