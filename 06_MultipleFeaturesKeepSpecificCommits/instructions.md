# Keeping the back half of a multiple feature change

This is a more difficult fix than keeping the front commits.

After the cycle resets, you will need to rebase the additional feature branch again, but you won't lose any work and you can do this without messing up too much history.

## Create the scenario

To create this scenario, create a "feature branch" off of `main`.  Once changes are in `main` you'll want to keep them so this is a pre-production rollout scenario. 

1. Create the feature branch named something like `multiplefeatures-keeplatter`.

1. From the feature branch, create two competing parallel teams or developer branches, i.e. `dev1` and `dev2`

    You will produce work by creating commits on both `dev1` and `dev2` branches.  

    >**Note:** To decrease complexity, do not have changes conflict. If you want extra practice, you can have a conflict.  You would have to resolve the conflicts during rebase of the `dev2` branch.

    Another thing to note is that the order doesn't really matter whether you merge `dev1` or `dev2` in this scenario, however this is because this scenario is contrived.

1. For ease, merge `dev1` first, then rebase `dev2` on the `multiplefeatures-keeplatter` branch, then merge `dev2`

    At the end of this operation, you should have a linear history that is all the commits for main, followed by all the commits for `dev1` and all the commits for `dev2`

    From an updated local `main` branch, run these commands:

    ```bash
    git checkout -b multiplefeatures-keeplatter
    git switch multiplefeatures-keeplatter
    git push -u origin multiplefeatures-keeplatter
    git checkout -b dev1
    git push -u origin dev1
    git switch multiplefeatures-keeplatter
    git switch -c dev2
    git push -u origin dev2
    ```
1. Move the two dev branches forward a couple of commits

    Use your local to create two commits on each branch and push to remote so that they are ready for `code review`.
    
1. For ease, merge `dev1` first, then rebase `dev2` on the `multiplefeatures-keeplatter` branch, then merge `dev2`

    Delete all the branches to simulate the real world and ensure rebase merge doesn't mess anything up

    When completed, you'll have a linear history with the new feature having around 4 commits (if you did 2 each).

    You'll then need to discern which commits to keep and which to remove.  Use `git diff <commitish> <commitish>` to see history comparisons or use the GUI of your choice to visualize differences across commits.

1. Checkout a new branch from the feature.  This will have identical commits.

    You will first need to create a copy of the feature changes as they exist with both features in the code.  This will ensure you don't lose anything and also ensure you have a failsafe in case something goes wrong.

    Create a full copy of the feature branch as `multiplefeatures-notpromoted`

    ```bash
    git switch -c multiplefeatures-notpromoted
    git push -u origin multiplefeatures-notpromoted
    ```  

1. Reset the `multiplefeatures-keeplatter` branch back to the base commit (last commit in main) so that the feature branch contains no new commits.

    Find the latest commit id of the main branch with `git log` or your tool of choice. Replace `<commitish>` in the command with the commit id:

    ```bash
    git switch multiplefeatures-keeplatter
    git reset --hard <commitish>
    ```  

    >**Information**: Don't run the following command unless you want to see it in action after resetting your feature branch to an earlier commit.  If you do run the command, make sure to reset the feature branch again so that you can complete the activity.

    ```bash
    git reset --hard origin/multiplefeatures-keeplatter
    ```  

1. Pick the commits for the feature you want to keep

    Use the tool of your choice and/or `git log` to determine what commit id's you need to keep.

    After determining which commits to keep, use a cherry-pick to pick those commits out of the chain.  You can pick multiple commits in the same operation, but you could *potentially* have more trouble with understanding conflict resolution if you do that.  For simplicity and to ensure everything works easily, for this demo I just pick them one at a time.

    >**Important**: Pick from the bottom (least recent) up (most-recent), taking the commit that was created least recently first, and the commit created most recently last.  

    ```bash
    git cherry-pick <commitish>
    ```  
    Repeat for each commit to pick.  Unless you run into a conflict, the cherry-pick operation should complete automatically.

1. Force push your changes into the remote

    Now that your branch has the desired feature in place and has removed the undesired feature, push the current branch out:

    ```bash
    git push --force-with-lease
    ```  

1. Reset the second branch to remove the commits that are in the primary feature branch

    Ensure there is no confusion by resetting the second branch.  Find the commit id that you need to reset to which will preserve the first feature(s) in the chain and remove the feature that was picked.

    ```bash
    git reset --hard <commitish>
    ```  

    >**Note:** If you are picking from the center, you would need to pick the remaining commits after resetting to the correct place before the feature that is being promoted.

1. Force push the non-promoted branch

    Push the non-promoted branch to ensure that your team won't have any confusion about trying to commit the feature a second time.

    ```bash
    git push --force-with-lease
    ```  

## Conclusion

In this activity, you saw how to pick and choose the features you want to keep and ensure that you don't lose any work in the process.
