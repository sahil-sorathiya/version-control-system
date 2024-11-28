# Setup

1. Ensure you have `cmake` installed locally
2. Run `./mygit.sh` to run your Git implementation, which is implemented in `src/Server.cpp`.

# Caution

1. The `mygit.sh` script is expected to operate on the `.git` folder inside the current working directory.  
   If you're running this inside the root of this repository, you might end up accidentally damaging your repository's `.git` folder.
   I suggest executing `mygit.sh` in a different folder when testing locally. For example: 
   ```bash
   mkdir -p /tmp/testing && cd /tmp/testing
   /path/to/your/repo/mygit.sh init
   ```
   To make this easier to type out, you could add a [shell alias](https://shapeshed.com/unix-alias/):

   ```bash
   alias mygit=/path/to/repo/mygit.sh
   mkdir -p /tmp/testing && cd /tmp/testing
   mygit init
   ```

1. The `mygit.sh` script is expected to run from the root directory of your repository only, otherwise it will give unexpected results.
   ```bash
   cd /path/to/root/of/repository
   ./mygit.sh <commands>
   ```

# Supported Commands

## init
Initializes a new Git repository in the current directory, creating a `.git` folder to track version history.
```bash
./mygit.sh init
```

## cat-file
Displays the content or type or size of a Git object by its hash.
```bash
./mygit.sh cat-file <option> <hash>
```
### Options
`-p` : Content  
`-t` : Object Type (Blob / Tree / Commit)  
`-s` : Size  

### hash
It must be from object directory.  
20 Bytes (40 Character) HEX hash.  

```bash
b72ab5b7815de62b37edbcf91501459c5151cb30
```
first two character = directory name in `/objects`.   
next 38 character = file name in that directory.  
For e.g.
```bash
.git/
 └── objects/
     └── b7/
         └── 2ab5b7815de62b37edbcf91501459c5151cb30
```

## hash-object
Compress the file convert it into `git object`.  
Calculates the SHA1 hash of an object and store it in the git object database ans returns its hash
```bash
./mygit.sh hash-object -w <file-path>
```

## ls-tree
Lists the contents of a given tree object
```bash
./mygit.sh ls-tree --name-only <tree-object-hash>
```

## write-tree
Creates a tree object from the current index (staging area) and returns its hash.
```bash
./mygit.sh write-tree  
```

## commit-tree
Creates a new commit object with a given tree object and commit message, and optionally a parent commit.
```bash
./mygit.sh commit-tree <tree-object-hash> <options>
```

### options
`-p` : parent hash of a tree object
`-m` : commit message

### example
```sh
./mygit.sh commit-tree <tree-object-hash> -p <parent-tree-object-hash> -m <commit-message>
```

## add
Stages changes (new or modified files) for the next commit.
```bash
./mygit.sh add <file-paths>
```

## commit
Records staged changes to the repository with a commit message.
```bash
./mygit.sh commit -m <commit-msg>
```

## checkout
Switches to a commit, updating the working directory to reflect that state.
```bash
./mygit.sh checkout <commit-object-sha>
```