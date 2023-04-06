# Remove a secret from history

You committed a secret password, encryption key, or Access/API token and you need to make it disappear.

Unfortunately, you're not really sure when this was committed to your team's repository, so you need to find the first commit and then rewrite your history from that point forward.

## Fork the Repo

If you're reading this, you likely already forked the repo, but make sure to fork the repo if you want to do this on your own based on my code and this walkthrough.

1. Fork it if you haven't already

    To do the remove a secret activity, [fork this repo](https://github.com/blgorman/RescueYourGitRepo-RemoveASecret.git):

    ```https
    https://github.com/blgorman/RescueYourGitRepo-RemoveASecret.git
    ```  

    Follow the instructions from below or in the README.md on that repo to complete the activity.

## Find the introduction point of the commit

To find the introduction of the commit, you'll need to use GIT bisect.  I recently wrote [a blog post on `git bisect` if you would like more information](https://training.majorguidancesolutions.com/blog/git-bisect-and-history-rewriting)  

1. Start the bisect operation to find the commit

    The repo has a number of commits.  I was trying to be vague in some of the messages so it isn't just obvious where the secret was introduced.  Also note that the secret is not real.  To connect to Azure Blob Storage I actually used the `usersecrets.json` file.  However, this contrived simulation is that some developer put the real key into the `appsettings.config` file.  The challenge is to find the last good commit and then we'll have to rewrite history from the next commit onward.

    The secret is introduced sometime after the first commit `4850171` and likely sometime before the `upload blob` commit `f5a892c`.  To narrow it down we could look at those as the good and bad commits.  To be safe, you could use the first and last commits.  The bisect operation is a binary search algorithm so it will move quickly to find the first bad commit.

    For each step, to validate if the commit is good or bad, look for the actual `key` in the `appsettings.config` file.

    Begin by running the following commands:

    ```bash
    git switch main
    git bisect start
    git bisect good 4850171
    git bisect bad f5a892c
    ```  

    You'll receive notification that there are three revisions left to test, roughly 2 steps.  This number will always depend on the size of your commit tree and also the span of the commits you enter as good and bad.

1. Test the file

    ```bash
    cat SimpleBlobStorageDemo/SimpleBlobStorageDemo/appsettings.json
    ```  

    You should see:

    ```json
    {
        "Storage": {
            "ConnectionString": "theconnectionstring"
        }
    }
    ```  

    This reveals that the current commit (b2b2398) does not have the connection string, so it is good.

    Enter the command:

    ```bash
    git bisect good
    ```  

1. The operation will automatically continue and you will see the next commit to test (444157c)

    ```bash
    cat SimpleBlobStorageDemo/SimpleBlobStorageDemo/appsettings.json
    ```  

    You should see:

    ```json
    {
        //"Storage": {
        //  "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=simplestor20251231blg;AccountKey=d2505129a437fb435fd83ec%7a9f762a3b51==;EndpointSuffix=core.windows.net"
        //}
    }
    ```  

    This is showing the connection string info, so it is bad

    ```bash
    git bisect bad
    ```  

1. The final test to check is commit d0829ce, and once again the operation automatically continues

    ```bash
    cat SimpleBlobStorageDemo/SimpleBlobStorageDemo/appsettings.json
    ```  

    You should see:

    ```json
    {
        "Storage": {
            "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=simplestor20251231blg;AccountKey=d2505129a437fb435fd83ec%7a9f762a3b51==;EndpointSuffix=core.windows.net"
        }
    }
    ```  

    This is also bad and needs to be cleaned up

    ```bash
    git bisect bad
    ```  

    The first bad commit is identified as:

    ```text
    d0829cebbecc8fc799b81a9da3a90eefa412b177 is the first bad commit
    commit d0829cebbecc8fc799b81a9da3a90eefa412b177
    Author: Brian L. Gorman ...
    Date:   Wed Apr 5 14:26:08 2023 -0500

        working on container storage code
    ```

    So now it is known that the first bad commit to fix starts with `d0829ce` and all the remaining commits will need to be purged of the connection string information.

    End the bisect operation and return to your main branch:

    ```bash
    git bisect reset
    ```  

    Alternatively, try running this command:

    ```bash
    git log -S "AccountKey=d2505129a437fb435fd83ec%7a9f762a3b51=="
    ```  
    
    What does it show?

## Remove the secret from the repository

1. Create a branch to store the original commits for visualization and as a security blanket

    ```bash
    git switch -c main-backup
    ```  

1. Reset local main to the 

    ```bash
    git switch main
    git reset --hard d0829ce
    git log --oneline
    ```  

1. Open the project using `code .` and fix the secret in the `appsettings.json` file so that it is no longer recorded

    Change the file to the following:

    ```json
    {
        "Storage": {
            "ConnectionString": "put-connection-string-in-user-secrets"
        }
    }
    ```

    Make sure to save the file and stage the changes with `add`.  Use `amend` to overwrite the initial commit with the new code.

    ```bash
    git add SimpleBlobStorageDemo/SimpleBlobStorageDemo/appsettings.json
    git commit --amend -m "working on container storage code"
    git log --oneline
    ```  

1. Now it's time to pick the rest of the changes

    You can pick the changes one-by-one.  There are likely to be a number of conflicts because the file could conflict now that the code is updated, so you'd have to resolve at every new commit where that file was changed.  For this reason, just pick them all and update the remaining commits by resolving the conflicts along the way as necessary.  In the real world if you need extra care, you may wish to make sure to review each commit individually.

    >**Note**: Make sure to NOT pick the commit that you just amended.  While you would be good just using your new code, all of that data is already in your history at this point and you're just adding complexity.  

    ```bash
    git cherry-pick 444157c..9728ac3
    ```  

    This brings up a conflict only at the last commit.  We got "lucky" here!

1. Use your merge tool of choice to resolve the conflict

    You can use VSCode, GitKraken, or Visual studio to resolve this conflict.  I've set VSCode to be my `mergetool` for this operation, so I'll use the command to launch it.  Any GIT tool would show you are in the process of a cherry-pick and you need to resolve a merge conflict.

    >**Note:** Setting VSCode as your mergetool can have consequences if you are in Visual Studio and encounter a conflict as it may get opened in VSCode instead of Visual Studio as you might expect.

    ```bash
    git mergetool
    ```  

    Note that you can abort: `git cherry-pick --abort` **Do this if you think there's a problem and you want to start over**
    Note that you can skip: `git cherry-pick --skip` **You should not do this!**
    Note the statement: "After resolving the conflicts ... `git add/rm <pathspec>`"

1. Resolve the conflict by keeping the version that doesn't have the connection string.  

    Complete the merge.

    ```bash
    git add SimpleBlobStorageDemo/SimpleBlobStorageDemo/appsettings.json
    ```  

    ```bash
    git cherry-pick --continue
    ```  

    If you can't get out, then run

    ```bash
    git commit --allow-empty
    ```  

    Set any commit message and finalize the operation (it *should* be the final commit message of `uncommented connection string`).

1. You've now rewritten history.  

    Try to see if anything exists in your repo that is bad

    ```bash
    git log -S "AccountKey=d2505129a437fb435fd83ec%7a9f762a3b51=="
    ```  

## Force push your new history into the remote

Everything is in place, so push the current history into the remote with a force-push

1. Complete the operation

    ```bash
    git push --force-with-lease
    ```  

1. Delete the backup branch

    ```bash
    git branch -D main-backup
    ```  

## Conclusion

In this walkthrough, you saw how to find and remove a bug or a secret from a repository and make sure that your secret is gone from the history forever.








    









