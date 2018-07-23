# Useful Commands!
```bash
### FUNCTIONS
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

### ALIASES 
alias daddy='sudo'  # nice
alias search='grep -rwn * -e'  # search for text in files within a dir
alias src='source ~/.zshrc'
alias ls='ls -G '  # color for mac, --color for linux
alias grep='grep --color=auto'
alias push='git push -u origin master'
alias pull='git pull'
alias gaa='git add --all'
alias 'gcn!'='git commit -v --no-edit --amend'  # modify most recent commit
alias force='git push -u -f origin master'
alias 'oops!'='gaa && gcn! && force'  # correct a fuck up w/o new commit
alias rc='vim ~/.zshrc'
alias shrink='export PS1="\u > "' # use when `pwd` is too long

```
