# Solving Merge Conflicts

## Introduction

Merge conflicts appear when your local changes and the changes on the remote conflict.
This can happen if changes are pushed to the repository while you are working on the same file/lines of code.
Most commonly, they appear when merging branches but for this exercise, you will not use them.
The goal of this exercise is to produce and solve a very simple merge conflict.

## (Optional) Resources for more information

http://allendowney.github.io/amgit/conflict.html

https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts (via branches)

## Setup

To create a merge conflict without using branches, two different git clients are needed.
For this exercise, you need to be grouped into pairs of two, decide who is partner A and B.
If you have not already, clone this repository into a local folder.

The exercises work by modifying a program template. Choose one to your liking, but both partners need to choose the same file.

- Python: python_template.py
- C#: cs_template.cs

## Producing the merge conflict

### 1.

Both A and B make sure that the local files are up-to-date via 'git pull'.

After this, **DO NOT PULL ON EITHER OF THE CLIENTS IF THE EXERCISE DOES NOT SAY SO!**

### 2.

Partner A implements the function "operation" by setting the value of result to ((a+b) * c)/d instead of 0.

Partner A then commits and pushes the changes.

### 3.
**AGAIN, DO NOT PULL JUST YET!**

Partner B implements the function "operation" by setting the value of result to (a\*c +  b\*c)/d instead of 0.

Partner B then commits and tries to push the changes.

## The merge conflict

> ! [rejected]        master -> master (fetch first)
> 
>error: failed to push some refs to 'https://github.com/NoRyyZ/GitHub-Workshop-Test.git'
>
>hint: Updates were rejected because the remote contains work that you do
>
>hint: not have locally. This is usually caused by another repository pushing
>
>hint: to the same ref. You may want to first integrate the remote changes
>
>hint: (e.g., 'git pull ...') before pushing again.
>
>hint: See the 'Note about fast-forwards' in 'git push --help' for details.

The push fails, since the remote already contains new changes to the same file from partner A.
GitHub does not solve the conflicts automatically!
The computer trying to push the local changes has to make sure that the remote changes are integrated before pushing.

**The following only applies to partner B:**

Get the remote changes via 'git pull'. Your command output should contain something like:

<details>
<summary>Python (click me!)</summary>

> CONFLICT (content): Merge conflict in python_template.py
> 
> Automatic merge failed; fix conflicts and then commit the result.

</details>

<details>
<summary>C#  (click me!)</summary>

> CONFLICT (content): Merge conflict in cs_template.cs
> 
> Automatic merge failed; fix conflicts and then commit the result.

</details>

Git tells you that it could not merge the remote changes automatically since they were made to the same region in the same file.
If they were in different files or different regions of the same file, git would probably perform the merge itself.

## Solving the merge conflict

When looking at the mentioned file, you'll see that git added information about the location of the changes:

<details>

<summary>Python  (click me!)</summary>

```py
def operation(a, b, c, d):
<<<<<<< HEAD (Current Change)
    result = (a*c +  b*c)/d
=======
    result = ((a+b)) * c)/d
>>>>>>> 9d32dca2134c25546c06c9e041549413ae722374 (Incoming Change)
    return result
```


Note the two sections. The first one

```py
<<<<<<< HEAD (Current Change)
    result = (a*c +  b*c)/d
=======
```

Shows the local changes that partner B wants to commit and push! The second one

```py
=======
    result = ((a+b)) * c)/d
>>>>>>> 9d32dca2134c25546c06c9e041549413ae722374 (Incoming Change)
```

Shows the changes that were uploaded to the repository while partner B was working on the same code.
Note that while the resulting value is the same, git cannot automatically decide which one to choose.

This has to be done manually by partner B.
For this, change the code such that it contains only one solution (also delete the lines added by git!).
Either enforce the local changes:

```py
def operation(a, b, c, d):
    result = (a*c +  b*c)/d
    return result
```

Or accept the remote changes:

```py
def operation(a, b, c, d):
    result = ((a+b)) * c)/d
    return result
```

Now add and commit the changes. After that, you can push to the remote.
</details>

<details>

<summary>C#  (click me!)</summary>

```cs
    public Int operation(Int a, Int b, Int c, Int d) {
<<<<<<< HEAD (Current Change)
        Int result = (a*c +  b*c)/d;
=======
        Int result = (a*c +  b*c)/d;
>>>>>>> 9d32dca2134c25546c06c9e041549413ae722374 (Incoming Change)  
        return result;
    }
```


Note the two sections. The first one

```cs
<<<<<<< HEAD (Current Change)
    Int result = (a*c +  b*c)/d;
=======
```

Shows the local changes that partner B wants to commit and push! The second one

```cs
=======
    Int result = (a*c +  b*c)/d;
>>>>>>> 9d32dca2134c25546c06c9e041549413ae722374 (Incoming Change)
```

Shows the changes that were uploaded to the repository while partner B was working on the same code.
Note that while the resulting value is the same, git cannot automatically decide which one to choose.

This has to be done manually by partner B.
For this, change the code such that it contains only one solution (also delete the lines added by git!).
Either enforce the local changes:

```cs
    public Int operation(Int a, Int b, Int c, Int d) {
        Int result = (a*c +  b*c)/d;
        return result;
    }
```

Or accept the remote changes:

```cs
    public Int operation(Int a, Int b, Int c, Int d) {
        Int result = (a*c +  b*c)/d;
        return result;
    }
```

Now add and commit the changes. After that, you can push to the remote.
</details>

## Git pull rebase

When the remote repo contains unaccounted changes, a normal git pull merges two states as if they existed at the same time.
If there are merge conflicts that can be resolved automatically, then git **automatically creates merge commits**.
Since this can cause unwanted behavior, there are two possible parameters:

> git pull **--ff-only**

Will only combine local changes and remote changes if no merging has to be done (Changes in different files, ...).
This will fail if files need to be merged (automatically or merge conflicts).

> git pull **--rebase**

Assuming the repo was in a state A before your changes B and the remote changes C, the resulting order in which changes are applied will be: A - C - B.
While this feels to be the cleanest solution, it can mangle the history and cause other problems.

When passing no parameter, the resulting behavior could be classified as something in between rebase and ff-only.

About git rebase: https://medium.com/@harishlyadav/when-to-use-git-rebase-explained-3c8192cba5c7

## Summary

Merge conflicts are created when the remote repository contains unaccounted changes during a push.
To solve them, you need to pull the changes and manually decide which should be in the final code push.
For larger files and changes, there can obviously be multiple merge conflicts and the manual merge can contain a mixture of accepting incoming changes or enforcing local changes.

While they cannot always be prevented, working on different files/code sections and communicating with each other can help reduce problems.
E.g. decide in your team which sections of code you will work on and maybe when you're pushing new changes.
Another helpful tool is git branching.

**Remember that git does not automatically check the remote for updates! Do small changes and pull/push once in a while. The longer you wait, the more likely you are to run into merge conflicts**