# `find` tricks

## `find` and `git`

* `git` moving all files of a specific type to the current directory:

```sh
$ find . -name '*.md' -exec git mv {} . \;
```

* removing all files except `.git` dir

```sh
$ find . -maxdepth 1 -name .git -prune -o -type d -print -exec git rm -r {} \;
$ find . -maxdepth 1 -name .git -prune -o -type d -print -exec rm -r {} \;
```

## find: listing all directories that are not child of a `.git` directory

```sh
# solution 1: using `prune`
$ find . -path '*/.git' -prune -o -type d -print

# solution 2: using predicate only
$ find . ! \( -path '*/.git' -o -path '*/.git/*' \) -type d

# solution 3: more portable and simpler than solution 2
$ find . -name .git -prune -o -type d -print
```

## find: prune search with multiple patterns

* excluding names `.git`, `.vim`, `.zplug` and `deps` from the search results

```sh
$ find . \
  -name .git -prune -o \
  -name .vim -prune -o \
  -name .zplug -prune -o \
  -name deps -prune -o \
  -type f \
  -print
```

## copy specific files preserving directory structure

* method 01: using `cpio` utility

```sh
$ find . -iname '*.iml' | cpio -pdm ~/code/testdir
```

* method 02: using `rsync`

```sh
$ find . -iname '*.iml' -exec rsync --relative {} ../anotherdir \;

# bonus: we can build upon the same idea with git-ls-files and xargs
$ git ls-files -iX .gitignore | xargs -I{} rsync --relative "{}" ../anotherdir
```

* method 03: using a `-exec` and `cp's` `--parents` option

```sh
$ find . -name '*.md' -exec cp --parents {} ../tricks-test \;
```

* finding all dot files on home directory:

```sh
$ ls -la `find ~ -maxdepth=1 -type l`
```

## opening in vim a set of files returned by a `find` search 

* use find and -exec

```sh
$ find . -name '*.txt' -exec vim {} +
```

* Or use a sub shell

```sh
$ vim $(find . -name '*.txt')
```

## find all files and directories `not` owned by a specific user

* use `-user` and the `!` flag

```sh
$ find . ! -user vagrant
```

## search for a class that is inside a jar in a dir full of jars. more info [here](http://stackoverflow.com/a/1343026/4921402)

```sh
$ find . -name "*.jar" -exec grep -Hsli ActiveTestSuite {} \;
./junit-4.12.jar
$ 
```
