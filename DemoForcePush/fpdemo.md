# Getting Started

To get started, Fork this repo at GitHub or clone to your local and push to your own repo at GitHub
this will ensure you have a repo that you have full control over to see the results of walking through this demo

## Create your `dev/feature` branch

First, you must create a branch at remote and pull it locally, or create a branch locally and push it.  Whether or not you are using trunk-based or some sort of GitFlow, just set your branch schema so that you can simulate other developers moving the trunk or dev branch forward.  For simplicity, this demo just utilizes `main` and your own branch.

1. Create a new `feature` branch as a developer

1. Ensure that you can push changes to it from your local development machine

## Create a few changes

Ensure that you make a couple of small commits and that you have some history on your branch both locally and at the remote.

1. Create a new file called `mychanges.txt` or something similar

1. Commit and push the changes

1. Add additional changes to the file, then commit and push.

## Create a change on `main`

To simulate that another team or another developer has changes that have been merged into the branch where you want to merge changes, create some simple changes to move the branch forward.

1. Make a couple of non-conflicting changes and commits.

    If you want extra practice with rebase, feel free to make a conflicting change.  You could then see how the conflict needs to be resolved around every rebased commit.

1. Ensure you moved forward at least one commit at remote.

    The simulation will require at least one commit moved to rebase your changes against.

1. Review the state of the repo

    If you use a tool like GitViz or you just use the `git log` command and review your changes and commit ids as compared in both main and your branch.

## Rebase your changes

Rebase your changes so that you will get the latest `main` commit history and then new commit(s) for your changes.

1. Get your local `main` branch up to date by fetching (not necessary to pull unless you are going to rebase on local)

    I usually rebase from remote, but rebasing from local is fine as well, as long as you've pulled the latest to your local.  For that reason, it's likely best to just pull `main` to get it up to date, so there is no confusion at all.

1. Rebase the changes on your local branch against `main`

    This rebase operation will cause your branch to rewrite commits on top of the latest `main` commit.  For this reason, you will orphan `n` commits (how many ever you are rebasing) and you will get `n` new commits.

1. When complete, run a `git status`

    At this point, you will see that your branch is different than remote with a statement like `your branch is "n" commits behind and "m" commits ahead of remote` followed by a bunch of solutions or statements about what you can do.

    >**Note:** Only run one of the following commands:

    Rebase from remote:

    ```git
    git rebase origin/main
    ```  

    Rebase from local:

    ```git
    git rebase main
    ```  

1. Try to push

    You will see that you cannot perform a normal `push` operation at this point.

## Force push your changes

Unless something crazy has happened, the reason your branch differs from what is at remote is due to the changed commit hashes, and not due to any changed work.

If you are concerned that you might have some differences, you *should* still be ok at this point, because you really care about what you have in your local more than what's at remote, right?  Along with that, you *will* have differences based on the commits that you rebased on top of, so any conflicts are resolved and this is actually what you want.

1. Force push your changes

    Run the following command to force push your changes

    ```bash
    git push --force-with-lease
    ```  

    Forcing with lease is safer than a straight force.  Unless you must really destroy some history, try to use the lease first.  Note that if you can't do it with a lease you may have some additional work to do to avoid losing changes as another operation has moved the branch forward and your force will lose something.

1. Run a status check to see that your code is now up-to-date.

1. Merge the code via a normal pull request.  

    >**Note:** the merge will be fast-forward so you won't get a merge commit since your history is just adding commits to the end of the main branch now

1. Additional practice

    Instead of merging, use a squash operation, which creates just one commit from your local branch, squashing all of your history down and making the commit history even more concise.

## Conclusion

In this little walkthrough, you learned about rebasing and then force-pushing your changes.

In the live demo, I've likely also talked about working with squash vs. straight merges.

## Next-steps

From here, learn about working with orphaned commits to see how you can recover what you had when before you changed your repo history.