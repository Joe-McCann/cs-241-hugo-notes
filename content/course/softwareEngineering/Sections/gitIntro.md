---
title: GTFO
linktitle: An Intro to Git and Github
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - cs241
    - software engineering
    - git

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1105
---

## An Intro to Git

Most people, in some capacity, have heard of `git`, or at least one of the many, many services that utilize `git`. Some of these services are such staples in the industry that I know that I personally didn't know that `git` was entirely seperate than Github the website! As such, I will teach you `git` first before mentioning some pieces of Github, all with the intention of making you able to contribute to open source projects.

Before continuing make sure you install `git` from [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

In order to see if its installed run

```bash
git --version
```

in your terminal, and get something like

```bash
$ git --version
git version 2.25.1
```

as the proper output. This will be with whatever version you install. Note that if you get that `git` is not recognized you may need to add it to your Path.

---

### What is Git?

Have you ever played the video game Skyrim, or a game with a similar manual saving mechanic? In that you are able to manually create a save file, and then whenever you load a save you can pick from one of your files. If you are a psychopath, then you will only have one save file and keep overwriting it every time you save. Imagine if at some point in the game you make a wrong decision, or encounter a bug that breaks a major quest, but you still overwrite your previous save. In that case if you wanna revert back to a not-shitty spot in the game, you will be screwed because you overwrote that save.

Instead of doing it the psycho method, just make a new save file whenever you save, so you can move back to a previous save at any point in time. This is the same basic idea of `git`. When you are working with code, it's nice to have these "checkpoints" that you can refer back to in case you royally screw something up.

`git` also has better functionality than just checkpoint saving though that makes collaboration super easy. Suppose I am working on some file, and my friend is working on a different file that is in the same project. We could both work on the files and keep adding checkpoints, but my friend is an idiot and will break the project with his changes.

I don't want to have to wait for him to fix his shit for me to test my code, so `git` has the ability for us to both work on our own individual copies of the project, and then once all of our changes are completed, we can merge our changes into the real project.

## Simple Stuff Working on this Website

In order to guide you through a tutorial, I am going to show you how to work on this website using the `git` command line. I am not giving you an in depth explanation of all commands, rather just how I use `git` to the degree that it works for me. As we progress I'll provide definitions to terms.

> **Definition**: A **repository** is a project folder that is managed by `git`. It is akin to a google drive project folder.

---

### Getting the Repository and Making a Branch

In your command line, `cd` over to a place you will want the website folder to be located in and run

```bash
git clone https://github.com/Joe-McCann/cs-241-hugo-notes.git
```

> **Definition**: **Cloning** is the act of taking a repository and copying it to another location. These two locations are able to link together so that changes in one can be added to the other. Repository copies on your device are called **local** repositories, and ones that are on a shared server are called **remote** repositories.

{{% callout warning %}}
If you are not listed as a collaborator of the repo you are cloning, you must first **fork** it which makes a copy under your account that is separate from the original repo.
{{% /callout %}}

Repositories that are hosted on services like Github are remote repos, that you can clone onto your local device. From there once you are done with all your changes you can then push said changes up into the remote.

When you first clone the repository, your version of the project will be on the primary version that is actually used on the website. When you make changes though, we don't want you to able to change that version willy-nilly as what if you prank me by changing my photo or something. As such, you need to be able to work on your own independent version that I will review when you are done. This independent version is called a branch.

> **Definition**: A **branch** is a version of the code that is independent from other branches. The primary default branch of the project is called the `main`[^1] branch.

As I said, I don't want you touching `main`, and if any of you touch `main` I swear to god you'll understand the memes about it[^2]. In order to make your own branch, you will run the command

```bash
git branch <name of your branch>
git checkout <name of your branch>
```

`git branch` will create the new branch, and `git checkout` will switch the branch you are working on. The shorter way for doing this I use is

```bash
git checkout -b <name of your branch>
```

If you'd like to see what branch you are currently on (as well as ones you have on your device), run

```bash
git branch
```

and there will be a \* next to the branch you are on.

---

### Saving Your Work

Now that you are on your own branch, you can actually start doing work, modifying the website, yada yada yada. When you want to save, you will have to commit your changes, this is like making a checkpoint in the games that we mentioned earlier! Now `git` is smart, and it will allow you to save specific files into the checkpoint if you want, so if you have 10 files modified, but only want 3 in the checkpoint, you can specifically add those to staged changes and they will be the only changes saved.

> **Definition**: In order to save any **staged** changes to the `git` history, you **commit** them.

In order to check all your current changes, run the command

```bash
git status
```

You will get an output that looks like

```git
D:\Coding_Files\cs-241-hugo-notes>git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   content/course/introToLogic/_index.md
        modified:   content/course/softwareEngineering/Sections/gitIntro.md

no changes added to commit (use "git add" and/or "git commit -a")
```

From this we see that we have no changes that are staged, so if we commit right now we'll save nothing. I want to save both of these files, so to add them to our staged changes we run

```bash
git add content/course/introToLogic/_index.md
git add content/course/softwareEngineering/Sections/gitIntro.md
```

Note there are faster ways to do this, I'm just showing every step for the purposes of explanation. Now if you `git status` you should see you changes are staged. From here we can save our changes with

```bash
git commit -m "A commit message explaining what this commit is. THIS IS REQUIRED"
```

The commit message can be anything but it is **not optional**.

From here you can continue to work on and commit to your branch until you are satisfied to push you **branch** up to the remote so that I can see your changes. To do this do

```bash
git push
```

and then cry because it won't work. You'll first encounter some permissions errors about tokens that I can never consistently figure out, so good luck on the internet with that. Then you will fail because your branch is new, but it will tell you what command to actually run in its place, so do that the first time.

[^1]: `main` is an updated term for the primary branch, previously the term was `master` branch. Should you see a branch called `master` just know thats an old term for primary branch.

[^2]: *"Joe, if you don't want us touching `main`, why do we have permission to do so?"*. Well first off I am lazy and don't want to set that up, but also at your jobs the odds are that they will not have safeguards in place to prevent you from working on `main`, so you gotta learn now to not do it for your own good.
