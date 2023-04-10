# Rescuing your GIT repository

This is the home base repo for my Rescuing your GIT repository talk.

Review the various folders for different scenarios.

Review the slides for some pictures of what I'm doing.

## Tools I'm leveraging in this talk

When I train GIT I like to use two main visualization tools:

1. GitViz

    - [Originally Found Here](https://github.com/Readify/GitViz) 
    - [.NET Core 6 version I've somewhat got working](https://github.com/blgorman/GitViz/tree/netcore2023)

1. Visualize GIT with d3

    - [Visualizing Git Concepts with D3](https://onlywei.github.io/explain-git-with-d3/)
    - [My fork - identical to original at this point](https://blgorman.github.io/explain-git-with-d3/)

1. VSCode
1. GIT Bash Terminal
1. GIT aliases

    ```text
    [alias]
        lol = log --oneline --graph --decorate --all
        expireunreachablenow = reflog expire --expire-unreachable=now --all
        gcunreachablenow = gc --prune=now
    ```  
1. VSCode as Default Editor

    ```text
    [core]
        editor = code --w
        autocrlf = true
    ```  

1. VSCode as MergeTool

    ```text
    [merge]
        tool = code
    [mergetool "code"]
        cmd = code --wait --merge $REMOTE $LOCAL $BASE $MERGED
    [mergetool]
        prompt = false
        keepbackup = false
    ```  

## General info and disclaimer

This is not the only way to accomplish these goals

1. If you have a better way

    Please let me know, I'd love to hear it

1. I'm not responsible for what you do to your repo

    This is guidance and training, and should not be used in production until you've mastered the material

## Repo for removing the secrets

1. The removing secrets setup is in another repo. 

    To do the remove a secret activity, [fork this repo](https://github.com/blgorman/RescueYourGitRepo-RemoveASecret.git):

    ```https
    https://github.com/blgorman/RescueYourGitRepo-RemoveASecret.git
    ```  

    Follow the instructions on that repo to complete the activity

## Additional Materials

Slides for the talk are [here](https://talkimages.blob.core.windows.net/rescuegit/RescueGit.pptx).  Lots of demo videos make it too large to host in a GIT repository at GitHub without GIT LFS.

## Conclusion

I hope this has helped you.  Please let me know if you have any questions or concerns, issues, etc.
