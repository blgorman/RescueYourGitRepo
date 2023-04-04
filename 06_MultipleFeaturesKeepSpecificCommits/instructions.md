# Keeping the back half of a multiple feature change

This is a more difficult fix than keeping the front commits.

After the cycle resets, you will need to rebase the additional feature branch again, but you won't lose any work and you can do this without messing up too much history.

## Create the scenario

To create this scenario, create a "feature branch" off of `main`.  Once changes are in `main` you'll want to keep them so this is a pre-production rollout scenario. 

1. Create the feature branch named something like `multiplefeatures-keepfront`.

1. From the feature branch, create two competing parallel teams or developer branches, i.e. `dev1` and `dev2`

    You will produce work by creating commits on both `dev1` and `dev2` branches.  

    >**Note:** To decrease complexity, do not have changes conflict. If you want extra practice, you can have a conflict.  You would have to resolve the conflicts during rebase of the `dev2` branch.

    Another thing to note is that the order doesn't really matter whether you merge `dev1` or `dev2` in this scenario, however this is because this scenario is contrived.

1. For ease, merge `dev1` first, then rebase `dev2` on the `multiplefeatures-keepfront` branch, then merge `dev2`

    At the end of this operation, you should have a linear history that is all the commits for main, followed by all the commits for `dev1` and all the commits for `dev2`

    From an updated local `main` branch, run these commands:

    ```bash
    git checkout -b multiplefeatures-keepfront
    git switch multiplefeatures-keepfront
    git push -u origin multiplefeatures-keepfront
    git checkout -b dev1
    git push -u origin dev1
    git switch multiplefeatures-keepfront
    git switch -c dev2
    git push -u origin dev2
    ```
1. Move the two dev branches forward a couple of commits

    Use your local to create two commits on each branch and push to remote so that they are ready for `code review`.
    
1. For ease, merge `dev1` first, then rebase `dev2` on the `multiplefeatures-keepfront` branch, then merge `dev2`

    Delete all the branches to simulate the real world and ensure rebase merge doesn't mess anything up

    When completed, you'll have a linear history with the new feature having around 4 commits (if you did 2 each).

    You'll then need to discern which commits to keep and which to remove.  Use `git diff commitish .. commitish` to see history comparisons or use a tool to visualize the changes.

1. Checkout a new branch from the feature.  This will have identical commits.

1. After determining which commits to keep, use a cherry-pick to pick those commits out of the chain.