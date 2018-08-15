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
## Environment Parameters for Writing Scripts
```bash
first_param=$1
second_param=$2  # and so forth
num_of_params=$#
all_parameters_passed_in_together=$* # like "arg1 arg2 arg3"
all_parameters_passed_in_separate=$@ # example below
$@='prod
test
dev'
# this is useful if you want to feed params into a for-loop

PID_of_parent_shell=$$
argument_of_prev_command=$_ 
exit_status_of_last_command=$?
PID_of_last_command=$! # example below, where i start a timer in the background + store its pid
start_timer & timer_pid=$!
# this is useful for starting a timer to activate a kill-switch in a program
```

## Some other favorites
These primarily help to navigate bash commands faster by reducing typing redundancy.

### Reuse last command
Simply typing `!!` will reexecute the most recently used command. This is particularly useful if you forgot to use `sudo`.  
You can also hit the up-key.

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

### Navigating Directories
```bash
cd - # Changes into the directory you were in previously
cd -2 # zsh exclusive, jumps to the directory you were in 2 cd's ago

pushd  /dir # pushes cwd onto stack and cd's into /dir
popd # pops head of stack and cds into new head
dirs # view stack

## Example ##
# Starting from $HOME
pushd /home/other_user # stack is now /home/other_user, $HOME, cwd is /home/other_user
popd # stack is no $HOME and cwd is $HOME
# One pushd and popd is equivalent to cd -
# You can make the stack as large as you want, so pushd popd is more powerful 
```

### Killing hard-to-kill processes
Suppose you run a pesky process that won't let you send `SIGINT` to end it. For example:

```bash
for f in `grep -rl * -e ":"`; do more $f; done
```
This command is great, **when used properly**. The `grep -rl * -e ":"` will return a list of filenames that can be reached recursively from the current directory, where the files include a certain pattern (in this case the colon ":" character). The for loop portion of the command will iterate through all of the files and execute the `more` binary executable. 

However, if you're like me, and you accidentally ran the command from the wrong directory.. you might be stuck traversing  **a ton** of files.. and good 'ole `CTRL-C` isn't helping. You could close your window and lose your shell session... or you can use this nice trick:

```bash
CTRL-Z
kill %%
```
`CTRL-Z` will **suspend** the currently running process. `%%` will select **the most recently suspended process**. Nifty huh?
