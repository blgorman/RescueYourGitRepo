# Remove a secret from history

You committed a secret password, encryption key, or Access/API token and you need to make it disappear.

Unfortunately, you're not really sure when this was committed to your team's repository, so you need to find the first commit and then rewrite your history from that point forward.

## Fork the Repo

If you're reading this, you likely already did so, but make sure to fork the repo if you want to do this on your own

1. Fork it

    To do the remove a secret activity, [fork this repo](https://github.com/blgorman/RescueYourGitRepo-RemoveASecret.git):

    ```https
    https://github.com/blgorman/RescueYourGitRepo-RemoveASecret.git
    ```  

    Follow the instructions from below or in the README.md on that repo to complete the activity.

## Find the introduction point of the commit

To find the introduction of the commit, you'll need to use GIT bisect.  I recently wrote [a blog post on `git bisect` if you would like more information](https://training.majorguidancesolutions.com/blog/git-bisect-and-history-rewriting)  

1. Start the bisect operation to find the commit


