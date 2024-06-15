# Getlet
## Internal Structures
Real Git distinguishes several different kinds of objects. For our purposes, the important ones are

!!! Blob
    The saved contents of files. Since Gitlet saves many versions of files, a single file might correspond to multiple blobs: each being tracked in a different commit.

!!! Tree
    Directory structures mapping names to references to blobs and other trees (subdirectories).

!!! Commit 
    Combinations of 

    - log messages
    - A reference to a tree
    - References to parent commits
    - Other metadata i.e. commit date, author, etc.
    
    The repository also maintains a mapping from branch heads to references to commits, so that certain important commits have symbolic names.
