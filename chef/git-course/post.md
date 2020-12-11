# A Git Course for the Absolute Beginner

Ever find yourself nervously laughing along when a colleague talks about how their push was rejected, so they radically performed a `git push --force`? Or pretend to relate to their sigh of relief after executing a `git rebase` with no conflicts? If so, help is just around the corner!

[Git](https://git-scm.com/) is easily the most popular version control system on the market. While previously stuck with server-based version control systems like [Subversion](https://subversion.apache.org/) or *(gasp)* Team Foundation Server, today's developer can enjoy the performance and flexibility of a *distributed* system like Git.

> Want to skip the details and learn more? Check out the new (and yes free!) course on Learn Chef: [Getting Started with Git](NEED URL).

## Advantages of Git

There is a reason, actually many reasons, why Git has become the phenomenon that it is today. After years of working with Subversion-like version control systems, dealing with laggy remote servers, checking-out/checking-in code, and never-ending conflicts, Git is a breath of fresh air.

For instance, Subversion forces each developer to use a working copy of code that points to a single remote (centralized) repository. Since Git is distributed, each developer downloads and maintains their *own local repository*. Each local repository contains a full history of all changes made to the code base. By virtue of its distributed nature, Git is faster, more reliable, and scales better.

### Git is Fast

![git commit image](git-commit.png)

Since everything about Git is local, the only speed limitations are generally your own computer! You don't need an active network connection to work with your code repository, perform diffs, or revert to previous versions of a file.

### Git is Reliable

![git merge image](git-merge.png)

Since Git is by nature distributed, it creates a more reliable environment. If a developer completely screws up their own repository, they can just clone the "master" branch and start fresh.

Likewise, a developer messing up something locally doesn't effect the other members of the team (as may happen with Subversion).

### Git Has Branching and Pull Requests

![git branch image](git-branch.png)

Arguably the most important feature of Git is its ability create *branches* of code. You can easily create separate, parallel, branches that are developed independently, and then *merged* together later.

Since every change in the code base may require its own branch, this also helps to ensure that the master branch only contains the most hardened and reliable version of the project's code.

![git pull request image](git-pr.png)

And how do you merge these distributed branches of code? Well that's done with *pull requests*. A pull request (or "PR" for short) allows you to request the ability to merge your branch into another repository. It facilitates not only a cleaner code base, but also allows for thorough commenting and documentation, critical for the modern app development team.

## Git or GitHub?

If you're confused about the difference between Git and [GitHub](https://github.com/), no worries. To be clear: Git is the actual version control system software that lets you keep track of your source code. GitHub, on the other hand, is a *service* that lets you manage one or more Git repositories online.

GitHub is almost entirely free to use and well worth taking advantage of!

> **TIP:** If you're averse to command-line tooling, GitHub also offers [GitHub Desktop](https://desktop.github.com/) for macOS and Windows.

## Learn Git on Learn Chef

All of this brings me to an important point: there is a new course on Learn Chef called [Getting Started with Git](NEED URL)!

> **NOTE:** Most of the courses on Learn Chef are DevOps-focused. Getting Started with Git is the first of a new series of courses that focus on related technologies and languages. Spoiler alert: Ruby, Powershell, and Bash are on the way! 🥳

*So what's in the course?*

**Getting to Know Git** – You'll get up and running (and productive) with Git as quickly as possible! Learn how to install Git, create a repository, add some commits, and pull some code changes.

**Diving Deeper into Git** - Once you're more familiar with the basics, it's time to learn about branching (like merging branches, branching strategy, alternatives to branching, and rebasing).

**Ready?** Check out [Getting Started with Git](NEED URL) now on Learn Chef! 🎓