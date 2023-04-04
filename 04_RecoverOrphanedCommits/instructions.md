

## Final

If you want to clean up any orphaned commits, run the following two commands. Note that this will clear your entire cache and these commits will be forever unretrievable:

```bash
git reflog expire --expire-unreachable=now --all
git gc –prune=now
```  