# I accidentally pushed to remote/main

You are having such a great day.  You had a good lunch (maybe it made you a little sleepy).  You create your feature branch but forget to switch to it.  Your coding can't be going any better and you are at a great place to push, so you do.

Oh no! You just pushed untested and unapproved changes into the main production branch and the build is running!

## Stop the build

Of course priority #1 is to stop any build if you can.  If you can't then I hope your team has some really great swapping in place so that your accidental code doesn't make it to production or doesn't stay there for long!

## Fix your issue

You have two options:

- Revert the commit(s), thereby triggering another build, but getting back to the codebase as it should be. You would really only need to do this if someone updated their local main or some process updated their copy of the repo to contain your commit(s).  Reverting keeps your changes but rolls forward so the history chain isn't broken.
- Keep your work and reset the main branch and force it back to where it should be.

## Pick & Reset

Assuming a revert operation is trivial enough, let's look at the pick & reset (similar to scenario 2)

### Get Set Up

The set up for this activity is easy:

1. Create a new feature branch

    First, create your feature branch `myfeature` but forget to switch to it/check it out

    ```bash
    git branch myfeature
    ```  

1. Create one or more commits 

    Create these commits directly on your local `main` because you think you are on your feature but you're actually on `main`

1. Push your changes

    You will know it's bad because you never had to set the upstream and/or a build will get triggered.  You'll also see the notice that you just pushed to `origin/main`

### Pick your commits

You already created your feature branch, so you can just pick the commits from `main`

1. Switch to your branch `git switch your-branch-name`

    Maybe you'll never forget to do this again

    ```bash
    git switch myfeature
    ```

    or

    ```bash
    git checkout myfeature
    ```  

1. Pick commits `git cherry-pick commitid`

    Easiest is just one-by-one.  You can do a range with `<committish> .. <committish>`.  You will have to resolve any conflicts 

    ```bash
    git cherry-pick 0aBcDe1
    ``` 

    Do this for all commits starting from the closest to the reset commit and working up the tree to the most recent commit

1. Switch to main

    Switch back to main:

    ```bash
    git switch main
    ```  

1. Reset main

    Determine the commit that was present before you created an issue.  Reset to the commit.

    ```bash
    git reset --hard main
    ```  

1. Force push to main

    You will need to rewrite history with a force push.  If you can't use a lease, you may need a full push here:

    ```bash
    git push --force-with-lease
    ```  

    or

    ```bash
    git push -f 
    ```   

    >**Note:** It is safer to try the --force-with-lease option first. If someone moved the commit history in the time you were doing this, you would not be able to complete the force.  If you are absolutely certain that the commit history needs to be YOUR version of main after removing your errant commits, you can use the `-f` flag to do a full force, which is extremely dangerous and can have massive side effects if you delete someone else's commit.

1. Your main branch is ok locally and at remote, but someone else might not be

    Your branch is ok, but in case someone else pulled main and has your errant commit on their history, they can run a hard reset command from remote to make sure to get the latest version (warning, this would also delete any work they did on their local main that is divergent from main):

    ```bash
    git switch main
    git reset --hard origin/main
    ```  
