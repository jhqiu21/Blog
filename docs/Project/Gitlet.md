# Getlet

This is the design documentation of project [Gitlet](https://sp21.datastructur.es/materials/proj/proj2/proj2) in [CS61b](https://sp21.datastructur.es/)


## Before Start

You should make sure you are familiar with concepts below.

Real Git distinguishes several different kinds of objects. For our purposes, the important ones are

??? Blob
    The saved contents of files. Since Gitlet saves many versions of files, a single file might correspond to multiple blobs: each being tracked in a different commit.

??? Tree
    Directory structures mapping names to references to blobs and other trees (subdirectories).

??? Commit 
    Combinations of 

    - log messages
    - A reference to a tree
    - References to parent commits
    - Other metadata i.e. commit date, author, etc.
    
    The repository also maintains a mapping from branch heads to references to commits, so that certain important commits have symbolic names.

??? Git 
    You can refer to my [Git Notes](#) of [Introduction to Git and GitHub](https://www.coursera.org/learn/introduction-git-github) by Google for more information.

Below are the basic technical skill used in this project:

??? Serialize
    A Java object is converted into a stream of bytes during **serialization** to be saved in a file or transferred over the internet. The serialized stream of bytes is transformed back into the original object during **deserialization**.

    - `static <T extends Serializable> T readObject(File file, Class<T> expectedClass)`: reads in a serializable object from a file.
    - `static void writeObject(File file, Serializable obj)`: writes a serializable object to a file

??? Java file IO operation
    There is a [Tutorial](https://www.tutorialspoint.com/java/java_files_io.htm) og IO operation in Java. 

## Structure

```
.gitlet
├─ HEAD
├─ objects
│  ├─ commit_1_id
│  ├─ commit_2_id
│  └─ ...
└─ ref
   └─ heads
      ├─ master
      └─ ...
```





## Classes and Data Structures




### Blob
#### Fields
```
private byte[] bytes;
private String id;
private String blobPath;
private File src;
private File blobSaveFileName;
```
Every blob object has its own saved contents of files. We get the file name and contents(byte array) from `src` and write the blob itself to file `blobSaveFileName`.

#### Useful Functions
- `static byte[] readContents(File file)`: reads in a file as a byte array
- `static File join(String first, String... others)`: joins together strings or files into a path.
- `static String sha1(Object... vals)`: In the case of blobs, “same content” means the same file contents. 

### Commit
#### Fields
```
private String message;
private String id;
private Date currentTime;
private String timestamp;
private List<String> parent;
private Map<String, String> blobRef;
private File commitSave;
```
**Remark**

- `id`: In the case of commits, it means the same metadata, the same mapping of names to references, and the same parent reference.
- `currentTime`: For initial commit, set its date `January 1, 1970, 00:00:00 GMT`. For following commits, get current time.
- `timestamp`: String generate from `currentTime`.
- `parent`: Use a list to store id of the last previous commit.
- `commitSave`: File get from commit `id`. We write commit object(deserializable) itself to this file.

#### Useful Functions
- `Date()`: Creates date object representing current date and time.
- `Date(long milliseconds)`: Creates a date object for the given `milliseconds` since January 1, 1970, 00:00:00 GMT.
- `SimpleDateFormat(String pattern)`: Constructs a SimpleDateFormat using the given `pattern` and the default date format symbols for the default `FORMAT` locale. Refer to [Javadoc](https://docs.oracle.com/javase%2F8%2Fdocs%2Fapi%2F%2F/java/text/SimpleDateFormat.html) for more information.


### Stage
#### Fields
```
private Map<String, String> blobRef = new HashMap<String, String>();
```
We implement a hash table storing reference of blob in add/remove stage. 
```
Key: path
Value: blob id
```

### Repository
#### Fields
```
public static final File CWD = new File(System.getProperty("user.dir"));
public static final File GITLET_DIR = join(CWD, ".gitlet");
public static final File OBJECT_DIR = join(GITLET_DIR, "objects");
public static final File REF_DIR = join(GITLET_DIR, "ref");
public static final File HEADS_DIR = join(REF_DIR, "heads");
public static final File HEAD_FILE = join(GITLET_DIR, "HEAD");
public static final File ADDSTAGE_FILE = join(GITLET_DIR, "add_stage");
public static final File REMOVESTAGE_FILE = join(GITLET_DIR, "remove_stage");
private static Commit commit; 
private static Stage addStage = new Stage();
private static Stage removeStage = new Stage();
```
**Remarks**

- `private static Commit commit` current commit.

## Commands

### `init`

- Creates a new Gitlet version-control system in the current directory. 
- This system will automatically start with one commit that contains no files and has the commit message `initial commit`.

### `add`

Adds a copy of the file as it currently exists to the staging area.

### `rm`
There are two cases:

- Unstage the file if it is currently staged for addition.
- If the file is tracked in the current commit, stage it for removal, remove the file from the working directory if the user has not already done so.
 
### `commit`

Here’s a picture of before-and-after commit after running following code:

```
add Blob4
rm Blob1
commit “add and rm”
```

=== "Before Commit"

    111

=== "After Commit"

    111


### `log`

- Starting at the current head commit, display information about each commit backwards along the commit tree until the initial commit.
- For merge commits (those that have two parent commits), only display first parent's information 

### `global-log`

- Displays information about all commits ever made
- The order of information does not matter.

### `find`

Prints out the ids of all commits that have the given commit message


### `status`

- Displays what branches currently exist, and marks the current branch with a `*`. 
- Displays what files have been staged for addition or removal.

### `checkout`

### `branch`

### `rm-branch`

### `reset`

### `merge`



## Persistence

## Reference
- [Gitlet Project Document](https://sp21.datastructur.es/materials/proj/proj2/proj2)
- 