## Find the commits

1. Use `reflog` and `log --oneline` to find commits that are "unreachable" or orphaned

    ```bash
    git log --oneline
    git reflog
    ```  

1. Note the commits that exist in the reflog that are not part of your commit tree

## Checkout an orphaned commit

Recover an unreachable commit by checking it out.  

1. Check out the commit by id, then get it back from detached by checking out a new branch

    ```bash
    git checkout <commitid>
    git checkout -b new-branch-name
    ```  

1. You are now able to work with the "lost" commit.

## Final

If you want to clean up any orphaned commits, run the following two commands. 

>**Note:** this action is **destructive** and will clear your entire cache and these commits will be forever **unretrievable**.

```bash
git reflog expire --expire-unreachable=now --all
git gc --prune=now
```  
