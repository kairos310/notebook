1. **Git's design:**
    - What is the primary difference between a distributed and centralized version control system (VCS)? Which of these best describes Git?
	    Git is a distributed system, 
	    CVS, perforce are centralized systems. 
    - What is a _local_ repository, and where is it stored in your local file system?
	    stored on the local file system
    - What is a _remote_ repository? Where are these typically hosted?
	    hosted on some cloud vcs like gihub or gitlab
    - What is a checksum, and how does Git use them?
	    a hash used to determine if a file has been changed
    - Be able to explain the different _states_ of files in a Git working tree:
        - tracked vs. untracked
	    tracked at files that github will record changes, untracked are files that aren't added, or are in gitignore
        - modified vs. unmodified
	        modified are files that git's checksum returns a different hash, indicating that it has been changed
        - staged
	        aka index
	        these are files that added with git add, ready to be committed, compared against modified
	        ![[Pasted image 20230911211815.png]]
        - committed
	        these are files that are added with a git commit, snapshot of staged changes 
1. **Git's components:**
    - What is a working tree?
	    the working tree is the directory where the local files are changed, associated with your repository
    - What is the Git staging area (index), and how is it used in the Git workflow?
	    the git staging area is the middle ground between what ou have done to your files, the working directory, and what you have last commited, the HEAD
    - What is HEAD?
	    these are the changes you have last commited, a reference to the commit object,, think of it as the current branch, whatever you are in your commit history, heads can point to commits only if it's the same commit taht a branch also points to, if not you have a (detatched head, making a commit in this state master will no longer move with you), or branch references, 
    - What is a branch?
	    represents an independent line of developement, pointers commits, typicaly the most recent in the branch
    - What is the "main" branch, and how is it different from other branches?
	    formerly known as the master branch, the default branch, main point of reference for stable code, typically merge features back to this branch
1. **Git's operations:**
    - How would you initialize (create) a _local_ Git repository in the following cases (that is, what Git commands would you use)?
        - _given_ a pre-existing remote repository
	        git clone url
        - _without_ a pre-existing remote repository
	        git init
		        defaults to main branch
		        git add 
		        git commit
	        git push
    - What is the basic Git "happy path" workflow? (You can assume no conflicts and all merges are "fast-forward" merges.)
	    git pull
	    this merges them
	    git add .
	    git commit -m ""
	    git push
    - Be able to describe the _purpose_ of the following Git commands, and show the _state_ of a Git local and remote repository after executing them ([that is, be able to _draw_ it like I did on the white board in class](https://rhodes.instructure.com/courses/5975/files/687444?wrap=1) [Download that is, be able to draw it like I did on the white board in class](https://rhodes.instructure.com/courses/5975/files/687444/download?download_frd=1)):  
        - git clone
        - git add
        - git commit
        - git pull
        - git push
        - git log
        - git status
    - What is a "fast-forward" merge? (Be able to draw this in terms of a local repository's commit history _before_ and _after_ the fast-forward merge.)
	    this is a merge that can be done automatically, like it's going forward in time (in terms of commits)
    - Describe how "git pull" is different from "git fetch".
	    git fetch tells the local repository that there are changes available in the remote repository without bring the changes into the local repository, git pull brings the copy of the remote directory changes into the local repository


