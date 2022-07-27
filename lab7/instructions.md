# Bonus lab: Version control & Git

This is an optional, bonus lab for a small amount of extra credit points (5% of a normal lab submission, or 0.1% of your grade). 
We'll cover just the basics of using the software `git` to track different versions of your software. This content will not be included in any 2110 tests or the final exam.

## Lab setup

For this lab, you can work in groups of up to 3 (join the same group on Canvas).

Create a new file called `lab7.txt` to enter some text-based responses for this lab.

## What is version control?

Version control is a technology that allows software engineers to manage different iterations of code and other files. Even when working alone, this can be useful for tracking changes, reverting to older versions of some software, and making sure you have a copy of your code if your computer breaks down. But the real power in using version control comes when working with teams of software engineers! Version control enables teams of people to work on the same codebase without overwriting each other's changes. Basic familiarity with version control will become increasingly useful to you after CS 2110, when you go on to work on larger, shared projects.

The core idea of every version control system is to group and track changes to files over time. Today, we'll focus on the most popular modern version control software, called `git`.

## `git` with it

`git` is a prolific version control manager, used by the majority of software engineering companies as well as research labs. You have already used GitHub to access these labs, [Github] is a software company that provides a web-based interface (and remote storage, also called "hosting") for projects using `git` (other hosting sites include [Gitlab] and [Bitbucket]). These hosting sites also manage shared access control and permissions, similar to Google drive sharing permissions (determining who can view and edit files).

Since `git` is such a useful skill, there are many good tutorials for learning Git, which we encourage you to read throughout and after this lab:
1. [Github tutorial][ght].
2. [Bitbucket tutorial][bht].
3. [Codeacademy (long)][cht].

[ght]: https://try.github.io/levels/1/challenges/1
[bht]: https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud
[cht]: https://www.codecademy.com/learn/learn-git

The core idea behind git is to think about individual, related groups of changes (called _commits_) that occur on multiple different directions of development (called _branches).

Here is a summary of core terminology:
- `repository` (or `repo`): a file-system directory/folder that holds a software project that is tracked by git.
> **Note**
> TODO: What is the name of the current repository where you are reading this file? Write it in `lab7.txt` after a `1. `.
- `commit`: a group of related changes to files. For example, a commit might include renaming a variable across multiple Java files. The commit would contain which lines changed, what the changes were, and some metadata (including a text description of what changed provided by the user).
- `branch`: a direction of software development. A repo has one default branch (usually called `main` or `master`) that contains the default, canonical version of the software. Engineers may make their own branches off of `main` (like trees branching off of a trunk) to try changes out a specific new direction (like a new feature) with a series of commits. Branches contain a sequence of commits. 
- `merge`: we promised that `git` is useful for software engineers working together! If two software engineers are working on branches that have conflicting changes, the engineers need to merge both branches to incorporate both changes in the default branch. The `merge` operation adds all the updates from one branch into another. Sometimes this can be done automatically, sometimes the user needs to provide input.
- `clone`/`push`/`pull`/`add`/`checkout`: these are all terms for interacting with different repos, commits, and branches. We'll get into them later in the tutorial.

## Making a Github account

If you have already made a Github account previously, feel free to use that instead of making a new account. If you are working in a group, you can make multiple accounts or just one.

If you have not already made a Github account, make one now. You can make one using your Cornell email address (see CS 3410's instructions [here][3410]) or your own personal email address. 

[3410]: https://www.cs.cornell.edu/courses/cs3410/2018fa/resources/username.html
[join]: http://github.com/join

> **Note**
> TODO: Write your Github username(s) (new or existing) on a new line in `lab7.txt` after a `2. `. If you want to keep your username private, just write `private username`.

## Cloning a repository

`git` allows many people to work on the same codebase by having shared repositories, or repos. For this lab, we'll have you _clone_, or create a local copy of, an existing repo.

Navigate to [this Github page][lab7] and click the green `Code` button in the upper right. Copy the URL for the repository from the menu that pops up. See the [instructions here][clone] for more details on the process of cloning from Github, including screenshots.

> **Note**
> TODO: Pick somewhere on your computer where you would live to save your local repo directory. Open up your terminal/command prompt, and type:

```
git clone https://github.com/avanhatt/cs2110lab7
cd cs2110lab7
```

> **Note**
> TODO: Then, rename what you want to call the remote repo from the default of `origin` to  `avanhatt`, to mark that this remote is Alexa's accounts remote:

```
git remote rename origin avanhatt
```

[lab7]: https://github.com/avanhatt/cs2110lab7
[clone]: https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository

Using your file navigator, you should not be able to see a new `cs2110lab7` directory/folder with two files in it: `Book.java` and `README.md`. The contents of these files match the contents shown for the remote repo hosted on Github.

### Aside: README

Public, shared codebases have the convention of having a README file that describes the project to the outside world. This README uses Markdown, a text annotation format used in these instructions as well (this provides the rich formatting, like the `Details` blocks).

# Creating your own remote repo

Since you want to track your own copy of any changes in this lab, we'll create a new _remote_ repo for you, in addition to Alexa's remote repo.

> **Note**
> TODO: Navigate to your Github profile (click the icon in the top right of any Github page, or go to [github.com][]). Click the green `New` button in the top left, and type in the name of this repo: `cs2110lab7` for `Repository name`. You can choose to make this either public or private (check the corresponding box). Click `Create repository`.

Now, we will connect your new empty, remote repo with your local repo. For this, you can follow the instructions that should now show on Github under `…or push an existing repository from the command line`.

> **Note**
> TODO: From your terminal/command prompt, make sure you are inside the directory/folder you created for the local repo. Follow Github's instructions to add an existing repo from the command line. You may need to follow steps to log in to your account. With those instructions, your remote copy will be called `origin`. 

# Making changes

Open `Book.java`. You can add this file to Eclipse, but for this lab, we would suggest selecting "link file" instead of "copy", since the file you edit needs to be within your repo's directory for `git` to track the changes. You can also open the file in any other text editor.

You'll see `Book.java` has a typo: the title of the example book is given as `The Clockwork Orange` instead of `A Clockwork Orange`.

In general, a good flow for making changes in `git` can be summarized as the follow (details after this high level list):

1. Make sure your local repo is up to date. We just cloned the repo, so this is true.
2. Make a series of related changes to file(s). Check changes with `status` and `log`.
3. Mark these files as ready to be committed, also known as staging, with `add`.
4. Create a new commit with `commit`.
5. Share your commit with others (and make sure you are saving a remote copy) with `push`.

> **Note**
> TODO: In your local repo, fix the typo in the book title described above. In your terminal/command prompt, run `git status` to see what changes have happened.

Now, we have some local changes. We mark them as ready to be committed with add. 

> **Note**
> TODO: In your terminal/command prompt, run:
```
git add Book.java
```

You'll see some information printed about the changes you have made.

Now, we have fixed a single, discrete issue: the typo. We can bundle up these changes into a commit to mark that they are ready to be saved as one related change. To pass the description of the commit, we'll use `-m` and then a string.

> **Note**
> TODO: In your terminal/command prompt, run (feel free to change the message):
```
git commit -m "Fix example title typo"
```

Now, we can save this change beyond your local repo by _pushing_ it to the shared repo. You don't have edit permission's on Alexa's repo, but do have full permissions on your own repo. 

You can see that your commit was successful by running `git log`, which shows the recent history of commits. You should see a commit with your new message listed.

> **Note**
> TODO: In your terminal/command prompt, run (feel free to change the message):
```
git push
```

You'll see a message that you need to specify where to push. Follow the instructions on pushing to your own remote (called `origin`), branch `main`.

You've now made some changes, described them concisely, and saved them safely off your own computer! 

# A bigger change

Now, let's imagine a slightly bigger change. 

> **Note**
> TODO: Update `Book.java` to include an author (a `String`) in addition to a title. Update the fields, constructor, and the example book.

> **Note**
> TODO: Update `README.md` to include a sentence stating what contents are included in a `Book` object.

Again, run `git status` to see both changes, then save these changes as a new commit.

> **Note**
> TODO: Follow the steps above to make and push a new commit, this time adding _both_ `Book.java` and `README.md`.

Again, check that your commit worked by running `git log`.

> **Note**
> TODO: Paste the description from `git log` showing both of your commits (you may need to scroll) to `lab7.txt` under `3.`.

# Combining others' changes

When working on code on a team, sometimes people make conflicting changes. Usually, teams try to avoid this by agreeing ahead of time on who should work on what file, but conflicting changes are bound to happen every once in a while.

`git`'s way of combining changes is with `merge`, which adds all of the changes from one branch's commit to another. See the externally linked tutorials for more details on this. 

We want to end this lab by giving you a little bit of experience with handling conflicting changes, also known as resolving a _merge conflict_.

## Alexa's changes

On Alexa's local copy of the repo, she was working on her own changes to add the year published to the `Book.java` file. She decided to work on a separate branch, which she created using `git checkout -b year`, which creates a new branch named `year` and switches the local repo to that branch. See the external tutorials for more details.

To see Alexa's changes in your own remote repo, you need to checkout a local copy.

> **Note**
> TODO: Get a local copy of Alexa's changes with the following, which specifies the name of the remote `avanhatt` and the branch on that remote `year`:
```
git checkout avanhatt/year
git checkout -b year
```

Now, let's try to merge Alexa's changes and your own. 

> **Note**
> TODO: Switch back to your changes with:
```
git checkout main
```

Now, attempt to merge Alexa's changes with:
```
git merge year
```

Oh no, a merge conflict! Because both branches/commits changed the same parts of the same file, such as the `Book` constructor, `git` does not know how to combine these changes on its own. Unlike with something like Google docs, `git` only shares code updates when the user explicitly pushes, not continuously as changes are made.

> **Note**
> TODO: In `lab7.txt`, under `4.`, add an explanation of why it would not be good for `git` to continuously sync changes like Google docs.

Now, let's have you actually manually resolve the merge conflict. 

> **Note**
> TODO: Open `Book.java` in your text editor or Eclipse and edit the code to include both new fields/updates. You will need to delete the code `git` adds to mark conflicts, such as `>>>>`, `<<<<`, and `====`. Keep editing until the file again compiles (you can check with jshell or with Eclipse). You can find more advice on this on Github [here][merge].

[merge]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-on-github

> **Note**
> TODO: Once you have resolved the merge conflict, follow the instructions in the `git` command to add the files you updated (`Book.java`) and run:
```
git commit
```

Note that git itself does not check that your code compiles, that responsibility is on you. 

> **Note**
> TODO: Run `git log` again and copy and paste your final merge commit under `5. `. Then, paste the entire contents of `Book.java` afterward, so you can turn in a single file.


# Basic usage and takeaways

If you are just using git/Github to save your own copies of your code, you do not need to worry so much about branches and merge conflicts. Your basic flow can just be to make changes, add them, commit them, and push to a single local repo. If you are working with just 1 partner on future course projects, you probably want to create a single remote repo and set up Github such that you both have permissions to push directly there, instead of having two separate remotes. You would then both push and pull from the same branch on the same remote repo.

Parts of this lab were structured this way to give you experience, not because they are the cleanest structure.

# Handing in this submission

That's it!

Your submission should include the following files:
- `lab7.txt`
   - Bullets 1-5
   - The final contents of `Book.java` pasted at the end.