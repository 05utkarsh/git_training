# git_training

---

An interactive git training meant to teach you how git works, not just which commands to execute.

Based on the general concept from Rachel M. Carmena's blog post on [How to teach Git](https://rachelcarmena.github.io/2018/12/12/how-to-teach-git.html) 

## This is currently a work in progress!

---

So, you want to use git right? 

But you don't just want to learn commands, you want to understand what you're using? 

Then this is meant for you!

Let's get started!

## Overview

In the picture below you see four boxes. One of them stands alone, while the other three are grouped together in what I'll call your _Development Environment_. 

![git components](img/components.png)

We'll start with the one that's on it's own though. The _Remote Repository_ is where you send your changes when you want to share them with other people, and where you get their changes from. If you've used other version control systems there's nothing interesting about that. 

The _Development Environment_ is what you have on your local machine. 
The three parts of it are your _Working Directory_, the _Staging Area_ and the _Local Repository_. We'll learn more about those as we start using git. 

Choose a place in which you want to put your _Development Environment_. 
Just go to your home folder, or where ever you like to put your projects. You don't need to create a new folder for your _Dev Environment_ though. 

## Getting a _Remote Repository_ 

Now we want to grab a _Remote Repository_ and put what's in it onto your machine. 

I'd suggest we use this one ([https://github.com/UnseenWizzard/git_training.git](https://github.com/UnseenWizzard/git_training.git) if for some reason you're not already reading this on github).

> To to that I can use `git clone https://github.com/UnseenWizzard/git_training.git`
> 
> But as following this tutorial will need you get the changes you make in your _Dev Environment_ back to the _Remote Repository_, and github doesn't just allow anyone to do that to anyone's repo, you'll best create a _fork_ of it right now. There's a button to do that on the top right of this page. 

Now that you have a copy of my _Remote Repository_ of your own, it's time to get that onto your machine. 

For that we use `git clone https://github.com/{YOUR USERNAME}/git_training.git`

As you can see in the diagram below, this copies the _Remote Repository_ into two places, your _Working Directory_ and the _Local Repository_. 
Now you see how git is _distributed_ version control. The _Local Repository_ is a copy of the _Remote_ one, and acts just like it. The only difference is that you don't share it with anyone. 

What `git clone` also does, is create a new folder wherever you called it. There should be a `git_training` folder now. Open it. 

![Cloning the remote repo](img/clone.png)

## Adding new things

Someone already put a file into the _Remote Repository_. It's `Alice.txt`, and kind off lonely there. Let's create a new file and call it `Bob.txt`. 

What you've just done is add the file to your _Working Directory_. 
There's two kinds of files in your _Working Directory_: _tracked_ files that git knows about and _untracked_ files that git doesn't know about (yet). 

To see what's going on in your _Working Directory_ run `git status`, which will tell you what branch you're on, whether your _Local Repository_ is different from the _Remote_ and the state of _tracked_ and _untracked_ files. 

You'll see that `Bob.txt` is untracked, and `git status` even tells you how to change that. 
In the picture below you can see what happens when you follow the advice and execute `git add Bob.txt`: You've added the file to the _Staging Area_, in which you collect all the changes you wish to put into _Repository_

![Adding changes to the staging area](img/add.png)

When you have added all your changes (which right now is only adding Bob), you're ready to _commit_ what you just did to the _Local Repository_. 

The collected changes that you _commit_ are some meaningful chunk of work, so when you now run `git commit` a text editor will open and allow you to write a message telling everything what you just did. When you save and close the message file, your _commit_ is added to the _Local Repository_.

![Committing to the local repo](img/commit.png)

You can also add your _commit message_ right there in the command line if you call `git commit` like this: `git commit -m "Add Bob"`. But because you want to write [good commit messages](https://chris.beams.io/posts/git-commit/) you really should take your time and use the editor.

Now your changes are in your local repository, which is a good place for the to be as long as no one else needs them or you're not yet ready to share them. 

In order to share your commits with the _Remote Repository_ you need to `push` them. 

![Pushing to the local repo](img/push.png)

Once you run `git push` the changes will be sent to the _Remote Repository_. In the diagram below you see the state after your `push`.

![State of all components after pushing changes](img/after_push.png)

## Making changes
So far we've only added a new file. Obviously the more interesting part of version control is changing files. 

Have a look at `Alice.txt`. 

It actually contains some text, but `Bob.txt` doesn't, so lets change that and put `Hi!! I'm Bob. I'm new here.` in there. 

If you run `git status` now, you'll see that `Bob.txt` is _modified_. 
In that state the changes are only in your _Working Directory_.

If you want to see what has changed in your _Working Directory_ you can run `git diff`, and right now see this: 

```Diff
diff --git a/Bob.txt b/Bob.txt
index e69de29..3ed0e1b 100644
--- a/Bob.txt
+++ b/Bob.txt
@@ -0,0 +1 @@
+Hi!! I'm Bob. I'm new here.
```

Go ahead and `git add Bob.txt` like you've done before. As we know, this moves your changes to the _Staging Area_. 

I want to see the changes we just _staged_, so let's show the `git diff` again! You'll notice that this time the output is empty. This happens because `git diff` operates on the changes in your _Working Directory_ only. 

To show what changes are _staged_ already, we can use `git diff --staged` and we'll see the same diff output as before. 

I just noticed that we put two exclamation marks after the 'Hi'. I don't like that, so lets change `Bob.txt` again, so that it's just 'Hi!' 

If we now run `git status` we'll see that there's two changes, the one we already _staged_ where we added text, and the one we just made, which is still only in the working directory. 

We can have a look at the `git diff` between the _Working Directory_ and what we've already moved to the _Staging Area_, to show what has changed since we last felt ready to _stage_ our changes for a _commit_. 

```Diff
diff --git a/Bob.txt b/Bob.txt
index 8eb57c4..3ed0e1b 100644
--- a/Bob.txt
+++ b/Bob.txt
@@ -1 +1 @@
-Hi!! I'm Bob. I'm new here.
+Hi! I'm Bob. I'm new here.
```

As the change is what we wanted, let's `git add Bob.txt` to stage the current state of the file. 

Now we're ready to `commit` what we just did. I went with `git commit -m "Add text to Bob"` because I felt for such a small change writing one line would be enough. 

As we know, the changes are now in the _Local Repository_. 
We might still want to know what change we just _committed_ and what was there before. 

We can do that by comparing commits. 
Every commit in git has a unique hash by which it is referenced. 

If we have a look at the `git log` we'll not only see a list of all the commits with their _hash_ as well as _Author_ and _Date_, we also see the state of our _Local Repository_ and the latest local information about _remote branches_. 

Right now the `git log` looks something like this: 

```ShellSession
commit 87a4ad48d55e5280aa608cd79e8bce5e13f318dc (HEAD -> master)
Author: {YOU} <{YOUR EMAIL}>
Date:   Sun Jan 27 14:02:48 2019 +0100

    Add text to Bob

commit 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e (origin/master, origin/HEAD)
Author: {YOU} <{YOUR EMAIL}>
Date:   Sun Jan 27 13:35:41 2019 +0100

    Add Bob

commit 71a6a9b299b21e68f9b0c61247379432a0b6007c 
Author: UnseenWizzard <nicola.riedmann@live.de>
Date:   Fri Jan 25 20:06:57 2019 +0100

    Add Alice

commit ddb869a0c154f6798f0caae567074aecdfa58c46
Author: Nico Riedmann <UnseenWizzard@users.noreply.github.com>
Date:   Fri Jan 25 19:25:23 2019 +0100

    Add Tutorial Text

      Changes to the tutorial are all squashed into this commit on master, to keep the log free of clutter that distracts from the tutorial

      See the tutorial_wip branch for the actual commit history
```

In there we see a few interesting things: 
* The first two commits are made by me.
* Your initial commit to add Bob is the current _HEAD_ of the _master_ branch on the _Remote Repository_. We'll look at this again when we talk about branches and getting remote changes.
* The latest commit in the _Local Repository_ is the one we just made, and now we know its hash.

> Note that the actual commit hashes will be different for you. If you want to know how exactly git arrives at those revision IDs have a look at [this interesting article ](https://blog.thoughtram.io/git/2014/11/18/the-anatomy-of-a-git-commit.html).

To compare that commit and the one one before we can do `git diff <commit>^!`, where the `^!` tells git to compare to the commit one before. So in this case I run `git diff 87a4ad48d55e5280aa608cd79e8bce5e13f318dc^!`

We can also do `git diff 8af2ff2a8f7c51e2e52402ecb7332aec39ed540e 87a4ad48d55e5280aa608cd79e8bce5e13f318dc` for the same result and in general to compare any two commits. Note that the format here is `git diff <from commit> <to commit>`, so our new commit comes second.

In the diagram below you again see the different stages of a change, and the diff commands that apply to where a file currently is. 

![States of a change an related diff commands](img/diffs.png)

Now that we're sure we made the change we wanted, go ahead and `git push`. 

## Branching

Another thing that makes git great, is the fact that working with branches is really easy and integral part of how you work with git.

In fact we've been working on a branch since we've started.

When you `clone` the _Remote Repository_ your _Dev Environment_ automatically starts on the repositories main or _master_ branch.

Most workflows with git include making your changes on a _branch_, before you `merge` them back into _master_. 
Usually you'll be working on your own _branch_, until you're done and confident in your changes which can then be merged into the _master_. 

> Many git repository managers like _GitLab_ and _GitHub_ also allow for branches to be _protected_, which means that not everyone is allowed to just `push` changes there. There the _master_ is usually protected by default. 

Don't worry, we'll get back to all of these things in more detail when we need them.  

Right now we want to create a branch to make some changes there. Maybe you just want to try something on your own and not mess with the working state on your _master_ branch, or you're not allowed to `push` to _master_.

Branches live in the _Local_ and _Remote Repository_. When you create a new branch, the branches contents will be a copy of the currently committed state of whatever branch you are currently working on. 

Let's make some change to `Alice.txt`! How about we put some text on the second line?  

We want to share that change, but not put it on _master_ right away, so let's create a branch for it using `git branch <branch name>`. 

To create a new branch called `change_alice` you can run `git branch change_alice`. 

This adds the new branch to the _Local Repository_. 

While your _Working Directory_ and _Staging Area_ don't really care about branches, you always `commit` to the branch you are currently on. 

You can think of _branches_ in git as pointers, pointing to a series of commits. When you `commit`, you add to whatever you're currently pointing to. 

Just adding a branch, doesn't directly take you there, it just creates such a pointer. 
In fact the state your _Local Repository_ is currently at, can be viewed as another pointer, called _HEAD_, which points to what branch and commit you are currently at. 

If that sounds complicated the diagrams below will hopefully help to clear things up a bit:

![State after adding branch](img/add_branch.png)

To switch to our new branch you will have to use `git checkout change_alice`. What this does is simply to move the _HEAD_ to the branch you specify.

![State after after switching branch](img/checkout_branch.png)

You'll notice that your _Working Directory_ hasn't changed. That we've _modified_ `Alice.txt` is not related to the branch we're on yet. 
Now you can `add` and `commit` the change to `Alice.txt` just like we did on the _master_ before, which will _stage_ (at which point it's still unrelated to the branch) and finally _commit_ your change to the `change_alice` branch. 

There's just one thing you can't do yet. Try to `git push` your changes to the _Remote Repository_.

You'll see the following error and - as git is always ready to help - a suggestion how to resolve the issue: 

```ShellSession
fatal: The current branch change_alice has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin change_alice 
```

But we don't just want to blindly do that. We're here to understand what's actually going on. So what are _upstream branches_ and _remotes_?

Remember when we `cloned` the _Remote Repository_ a while ago? At that point it didn't only contain this tutorial and `Alice.txt` but actually two branches. 

The _master_ we just went ahead and started working on, and one I called "tutorial_wip" on which I commit all the changes I make to this tutorial. 

When we copied the things in the _Remote Repository_ into your _Dev Environment_ a few extra steps happened under the hood. 

Git setup the _remote_ of your _Local Repository_ to be the _Remote Repository_ you cloned and gave it the default name `origin`. 

>Your _Local Repository_ can track several _remotes_ and they can have different names, but we'll stick to the `origin` and nothing else for this tutorial. 

Then it copied the two remote branches into your _Local Repository_ and finally it `checked out` _master_ for you. 

When doing that another implicit step happens. When you `checkout` a branch name that has an exact match in the remote branches, you will get a new _local_ branch that is linked to the _remote_ branch. The _remote_ branch is the _upstream branch_ of your _local_ one. 

In the diagrams above you can see just the local branches you have. You can see that list of local branches by running `git branch`. 

If you want to also see the _remote_ branches your _Local Repository_ knows, you can use `git branch -a` to list all of them.

![Remote and local branches`](img/branches.png)

Now we can call the suggested `git push --set-upstream origin change_alice`, and `push` the changes on our branch to a new _remote_. This will create a `change_alice` branch on the _Remote Repository_ and set our _local_ `change_alice` to track that new branch. 

> There is another option if we actually want our branch to track something that already exists on the _Remote Repository_. Maybe a colleague has already pushed some changes, while we were working on something related on our local branch, and we'd like to integrate the two. Then we could just set the _upstream_ for our `change_alice` branch to a new _remote_ by using `git branch --set-upstream-to=origin/change_alice` and from there on track the _remote_ branch.

After that went through have a look at your _Remote Repository_ on github, your branch will be there, ready for other people to see and work with. 

We'll get to how you can get other people's changes into your _Dev Environment_ soon, but first we'll work a bit more with branches, to introduce all the concepts that also come into play when we get new things from the _Remote Repository_. 

## Merging

As you and everyone else will generally be working on branches, we need to talk about how to get changes from one branch into the other by _merging_ them. 

<!-- NO CONFLICT -->
We've just changed `Alice.txt` on the `change_alice` branch, and I'd say we're happy with the changes we made. 

If you go and `git checkout master`, the `commit` we made on the other branch will not be there. To get the changes into master we need to `merge` the `change_alice` branch _into_ master. 

Note that you always `merge` some branch _into_ the one you're currently at. 

### Fast-Forward merging

As we've already `checked out` master, we can now `git merge change_alice`. 

As there are no other _conflicting_ changes to `Alice.txt`, and we've changed nothing on _master_, this will go through without a hitch in what is called a _fast forward_ merge. 

In the diagrams below, you can see that this just means that the _master_ pointer can simply be advanced to where the _change_alice_ one already is. 

The first diagram shows the state before our `merge`, _master_ is still at the commit it was, and on the other branch we've made one more commit. 

![Before fast forward merge](img/before_ff_merge.png)

The second diagram shows what has changed with our `merge`.

![After fast forward merge](img/ff_merge.png)

### Merging divergent branches

Let's try something more complex. 

A some text on a new line to `Bob.txt` on _master_ and commit it. 

Then `git checkout change_alice`, change `Alice.txt` and commit. 

In the diagram below you see how our commit history now looks. Both _master_ and `change_alice` originated from the same commit, but since then they _diverged_, each having their own additional commit. 

![Divergent commits](img/branches_diverge.png)

If you now `git merge change_alice` a fast-forward merge is not possible. Instead your favorite text editor will open and allow you to change the message of the `merge commit` git is about to make in order to get the two branches back together. You can just go with the default message right now. The diagram below shows the state of our git history after we the `merge`.

![Merging branches](img/merge.png)

The new commit introduces the changes that we've made on the `change_alice` branch into master. 

As you'll remember from before, revisions in git, aren't only a snapshot of your files but also contain information on where they came from from. Each `commit` has one or more parent commits. Our new `merge` commit, has both the last commit from _master_ and the commit we made on the other branch as it's parents.

### Resolving conflicts

So far our changes haven't interfered with each other. 

Let's introduce a _conflict_ and then _resolve_ it. 

Create and `checkout` a new branch. You know how. I've called mine `bobby_branch`.

On the branch we'll make a change to `Bob.txt`. 
The first line should still be `Hi!! I'm Bob. I'm new here.`. Change that to `Hi!! I'm Bobby. I'm new here.`

Stage and then `commit` your change, before you `checkout` _master_ again. Here we'll change that same line to `Hi!! I'm Bob. I've been here for a while now.` and `commit` your change. 

Now it's time to `merge` the new branch into _master_. 
When you try that, you'll see the following output

```ShellSession
Auto-merging Bob.txt
CONFLICT (content): Merge conflict in Bob.txt
Automatic merge failed; fix conflicts and then commit the result.
```
The same line has changed on both of the branches, and git can't handle this on it's own. 

If you run `git status` you'll get all the usual helpful instructions on how to continue. 

First we have to resolve the conflict by hand. 

> For an easy conflict like this one your favorite text editor will do fine. For merging large files with lots of changes a more powerful tool will make your life much easier, and I'd assume your favorite IDE comes with version control tools and a nice view for merging. 

If you open `Bob.txt` you'll see something similar to this (I've truncated whatever we might have put on the second line before): 

```
<<<<<<< HEAD
Hi! I'm Bob. I've been here for a while now.
=======
Hi! I'm Bobby. I'm new here.
>>>>>>> bobby_branch
[... whatever you've put on line 2]
```

On top you see what has changed in `Bob.txt` on the current HEAD, below you see what has changed in the branch we're merging in.

To resolve the conflict by hand, you'll just need to make sure that you end up with some reasonable content and without the special lines git has introduced to the file.

So go ahead and change the file to something like this: 

```
Hi! I'm Bobby. I've been here for a while now.
[...]
```

From here what we're doing is exactly what we'd do for any changes. 
We _stage_ them when we `add Bob.txt`, and then we `commit`. 

We already know the commit for the changes we've made to resolve the conflict. It's the _merge commit_ that is always present when merging. 

Should you ever realize in the middle of resolving conflicts that you actually don't want to follow through with the `merge`, you can just `abort` it by running `git commit --abort`. 

## Rebasing

<!-- rebase TWO BRANCHES -->

<!-- NO CONFLICT -->

<!-- RESOLVE A CONFLICT -->

## Updating the _Dev Environment_ with remote changes

<!-- fetch -->

<!-- pull -->

<!-- pull -r -->

## Cherry-picking

<!-- cherry pick from a branch -->

## Rewriting history

<!-- ammending -->

<!-- squashing -->

<!-- force pushing -->