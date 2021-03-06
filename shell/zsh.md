# ZSH tricks

## ZSH conditional expressions

* `-z`: testing if a shell variable is set

```zsh
# `-z` returns true if string's length is equal to 0.
if [ -z "$HISTFILE" ]; then
    HISTFILE=$HOME/.zsh_history
fi
```

## ZSH Globbing

* more tips can be found [here][1].

```sh
# In zsh, there are |glob qualifiers| and |string modifiers|.

# Here's an example of a |string modifier| 
# string modifiers are always separated by a colon `:`
# `a` = full qualified name
$ print file.txt(:a)

# `A` = like `a`, but resolves symlinks
$ print ~/.vimrc(:A)

# Here's an example of a |glob qualifier| 
# D = include dotfiles 
# / = only directories
print -l ^.git(D/)

# here's an example of a |glob qualifier| and a |string modifier| together. 
# the modifier is always separated by a colon `:`.
$ print ^(.git*)(D.:a)
```

* show all files including 'dot' files that are not named .git:

```sh
# D = include dotfiles;
# . = only files
$ print -l ^.git(D.)
```

* removing all directories except some: `rm -rf ^(vim-colors-solarized|vim-airline)`

* globbing inside a shell function

```sh
fullqualifiedname() {
  print -l ${1:a}
}
```

## zsh errors

* `function definition not found` error
```sh
# simply remove all .zcompdump* files from ~
$ rm -f ~/.zcompdump*
```

[1]: <http://reasoniamhere.com/2014/01/11/outrageously-useful-tips-to-master-your-z-shell>
# Installing zsh on OSx
## Installing from source

* dependencies

```sh
$ sudo apt-get install -y git-core gcc make autoconf yodl libncursesw5-dev texinfo checkinstall
``` 

* installing

```sh
# Make configure
./Util/preconfig

./configure
make
make check
make install
```


## if git completion is off, follow this steps

* theres a variable called `$fpath`
* this variable contains the path to bootstrap functions, completions, etc...
* `print -l $fpath` will list the directories on $fpath 
* in my case, I had these:

```sh
$ /usr/local/share/zsh/site-functions
$ /usr/local/Cellar/zsh/5.0.8/share/zsh/functions
```

* in the first one, site-functions, I verified that there were 2 wrong symbolic links:
  * tinha um `_git` symlink e um link para um bash completions. retirei esses links e funcionou perfeitamente.
  * a `_git` symlink and a link to a bash completion. I removed these and everything worked like a charm.
