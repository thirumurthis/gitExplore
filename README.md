Git command:

> git commit -am "commit comment"
-- above command will add and commit the changes.


> git branch -d branch_name_to_be_deleted. 
--- above command from different branch (this will not delete if there are any commmits 
present in the deleting branch, the command will recommend to use -D in thsi case )

> git push -d origin brach_name_to_be_deleted

prune stale branch = remote tracking branch (not remote branch)

Stale branch = a remote tracking branch that no longer tracks anything 
because that acutal branch in the remote repository has been deleted.

there are three remote branches:
  1. branch on the remote repository (somebranch)
  2. local snapshot of the remote branch (origin/somebranch)
  3. local branch, tracking the remote branch (somebranch)
  
  depiction:
  remote branch |   local snapshot ( called as remote tracking branch) | local branch 
  
  git fetch:
    from remote repository
    sync up the tracking branch <- origin/somebranch
    which is handy because it makes the tracking and allows to work offline.
    
    git branch -D branchName -> deletes the branch #3 (local) 
    git push -d origin branchName -> deleted the branch #1 (remote)
    
    what happens to #2 the local remote snapshot, the git push -d .. command will prune the  #2.
    
    it is necessary to delete the branches when collaborators (other developers who uses the branch) delete branches
      > if the collabrator is going to delete the #1 the remote branch.
      > then the tracking branch in our project will be out of sync.
      > Note Fetch doesn't prune automatically in this case.
      This needs to be done by the us(developer in local branch) using
      #to Delete the stale remote-tracking branches
      > git remote prune origin
        or any other remote repository using instead of origin
        
        --dry-run option will be display what it does before what it actually does,
        
    > git branch -r (will provide the branch information in the remote snapshot area, since the example
    "someotherbranch" created and pushed to the remote from the local repository didn't display
    someotherbranch, which is used by another developer. when issuing the command.
     we need to use the command > git fetch, in order to reflect that branch.
     
     fetch & prune:
      > git fetch --prune
      use global configuration,
         we can set the global config to prune automatically when issuing fetch not a recommended option 
         because it may delete something accidently.
         $ git config --global fetch.prune true
     
     $ git prune - prunes all the local stale branch. this is different from git remote prune
     $ git gc  (git prune is part of garbage collector of gc)
     
     TAGs:
       lightweight tag: 
                git tag Name_of_the_Tag reference_of_the_tag
       annotated tag: (use this most commonly)
                git tag -a <version or name> -m <comment> reference_of_the tag
                we can use the -am , will open the editor.
                if the -a is not provided and include the -m, then git must add the annoatated tag.
                it is best practice to use -a.
                - if we omit the reference the git will use the current HEAD.
                > git tag -am "start" v1.0 asadsadada121
                 
       > git tag --list (will list)         
       > git tag -l
       > git tag -l "v*"
       
       List the tags and the annoation
       > git tag -l -n 
       > git tag -ln
       
       like working witht SHAs number
       > git show tagname
       > git diff tag1..tag2
      
  Delete tag
  > git tag --delete v1.0
  > git tag -d v1.0
  
  push tags to remote:
    all the tags created are local
    git push, cannot push the tag to remote, does not transfer tags. all tags are only local
    if we want to transfer, we need to do explicitly.
    
    > git fetch will fetch the tags from remote that are shared to remote. but not pushs the local tag.
    
    > git push origin tagname 
    > git push origin --tags 
       - the above --tags will push all the tags.)
    > git fetch  -> will fetch every shared tags from remote.
    > git fetch --tags  -> will fetch only tags with necessary commits
    
    delete remote tags.
    > git push origin :tagName  => older version
    > git push --delete origin tagName  => intermidiate version
    > git push -d origin tagName > intermidiate version
    
    checking out tags:
    
    Tags are not same as branch. Branch are able to commit and transfer between it.
    
    to checkout a tag is to using a new branch
      > git checkout -b new_branch v1.0
      
   we can use the (not recommended implicatons to this below command usage) 
      > git checkout v1.0
      
   Detached HEAD state => branch without name.
   garbage collected two weeks by git
   
   usually the HEAD will be pointed to the master branch.
   
   if you wanted to checkout a commit, the HEAD will be moved to that node.
   if there is no name ot the branch or node it will be a detached head.(unnamded branch)
   
   tag the commit. (HEAD detached)
     > git tag temp
   create a branch (HEAD detached)
     > git branch temp_branch
   
   create a branch and reattach HEAD (more preferred way, best practice)
     > git checkout -b temp_branch
     
     git checkout will move the HEAD pointer to the appropriate branch/node.
    
    intereactive mode:
     git add -i
    Hunk => difference between the changes made in the same file.
    
    patch -> git add -i
    > p 
    provides following option (y, n, q, a, d, /, j , J, g, s, e,?) 
    y - yes 
    n - no
    s -split
    ? - help
    
    shortcut is, > git add -p
    
    options provided to either add to the chached area, y or n.. etc. option
    
    split a hunk:
       Required one or more unchanged lines between lines.
     
     git add -i
     
     > p 
     
     > from option : s
     granular
     
     git diff --cached
     git show
     git log
     
     Edit hunk:
     most useful when the hunk cannot be split automatically
     Diff-style line prefixes: +, - , # , space
     
     
     git diff
          adds a space in the screen
     which is useful to edit the hunk
     
     git add -i
     
     > p 
     
     > e
      opens up a text editor
      
  Cherry-picking commits:
      
      > Apply the changes from one or more existing commits
      > Each existing commit is recorded as a new commit on the current branch
           -tell git to get the commit grab it and apply to this branch
      > Similar to copy and paste - history are different (which will had different  SHA)
      SHA is created by git for different commit.
      
      if you are creating a new branch for new feature and the master is ahead of few commits.
      using cherry pick we can get the future commmit in the current branch. the commit will have a different SHA but reference the same changes.
      
      > git cherry-pick deaesaa121
      > git cherry-pick asdads121..asdasd123
      SHA deaesaa121 (identifier of commit)
      .. used for specifying range of commit
      
      Note: Cannot cherry-pick merge commits
      -e , --edit to edit message
  conflicts
     git cherry-pick --continue
     
     
     create diff patches
     >  echo "hello" > temp.txt
     
     > git diff commit1 commit2 > output.diff
     .diff - is the extension to be used for difference file
     
     > git log --oneline
     
     diff files can be shared with others
     this can be applied to the our working directory. how to do it?
     
     diff file doesn't have any history, this is a set of changes, we need to manually issue commit
     
     to apply the diff patch
     
     > git apply output.diff
     output.diff is the file name of the patch
     
how to create a branch from the commit
     
     > git checkout -b branchName <commit SHA name>
     
     > git checkout -- fileName 
      this will clear and reset to the initial changed commit file in the branch
      
      
     
     
     
      
      
     
     
     
     
     
   
   
   
  
       
       
       
                
                
         
     
  
      
      
    
  
  
