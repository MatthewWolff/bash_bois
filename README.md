# Useful Commands!

### Functions

```bash
addalias()
{
 new_alias="alias $(echo $1 | sed -e "s/=/='/" -e "s/$/'/")"
 echo $new_alias >> ~/.zshrc
 source ~/.zshrc
}

sublime()
{
  open "$@" -a "/Applications/Sublime Text.app"
}
pycharm()
{
  open "$@" -a "/Applications/PyCharm.app"
}
cd()
{
  builtin cd $@ && ls;
}
hfs()
{
  hadoop fs -$*
}
settheme() # ohmyzsh only
{
  sed -i '' -e "s/ZSH_THEME=\"[a-z]*\"/ZSH_THEME=\"$1\"/" ~/.zshrc
  source ~/.zshrc
}

```


### Aliases 
```bash
alias daddy='sudo'  # nice
alias search='grep -rwn * -e'  # search for text in files within a dir
alias src='source ~/.zshrc'
alias ls='ls -G '  # color for mac, --color for linux
alias grep='grep --color=auto'
alias push='git push -u origin master'
alias pull='git pull'
alias gaa='git add --all'
alias 'gcn!'='git commit -v --no-edit --amend'  # retroactively commit files to last commit
alias force='git push -u -f origin master'
alias 'oops!'='gaa && gcn! && force'  # correct a fuck up w/o new commit
alias rc='vim ~/.zshrc'
alias shrink='export PS1="\u > "' # use when `pwd` is too long

```

## Some other favorites
These primarily help to navigate bash commands faster by reducing typing redundancy.

### Reuse last command
Simply typing `!!` will reexecute the most recently used command. This is particularly useful if you forgot to use `sudo`.

### Reuse specific arguments from last command
Suppose you want to quickly `cat` a file to see its output, or you copy a file to another directory with `cp <file> <otherDir/file>`. Immediately after seeing its contents or copying it over, you want to edit the file with your favorite text editor. 

With the following special syntax, you can pull a command-line argument from a previous command. 

**Syntax:**
`!:N` where N is any integer

**Usage:**
```bash
cat ~/Projects/Work/file.txt
vim !:1  # 0-based indexing
```

**NOTE:** `vim !:1` is identical to `vim ~/Projects/Work/file.txt` and will only work if these commands are executed in EXACTLY this order. 

### Reusing a command that is older than the most recently one
If the command you want to pull from was executed more than 1 command ago, and you **know** how many executions ago it was, you can use the following syntax `!-2` (if you want to re-execute the 2nd most recent command)... or `!-2:1` to pull the 1st argument from 2 commands ago.

### Reuse a range from a command
`!:1-2` will get a range of arguments, which can be useful if you forgot a particular flag in the first execution but entered all of the other ones correctly. 

### Overall generic syntax
`!-N:X-Y` will retrieve a range of arguments between X and Y (0 based and inclusive) from the command used N executions ago. 

