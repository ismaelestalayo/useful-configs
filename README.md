# Powershell

Currently using the PowerShell 7.1 preview (from store for auto updates) which I've also customized to be inline with my zsh prompt.

1- Install necessary modules:

```Powershell
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

2- Then create/edit a profile.ps1 with `vim $PROFILE` (you can use notepad as well) and paste the following:

```Powershell
Set-PSReadLineOption -EditMode Emacs
Import-Module posh-git
```

3- Modify your prompt

```Powersshell
# Add a new line before and after your prompt
$GitPromptSettings.DefaultPromptSuffix = '`n▶ '
$GitPromptSettings.DefaultPromptPrefix = '`n'
```


*(optional)* If there's any problem with execution policies of the script at launch:

```Powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

----

# bash

Enable case-insensitive auto-completion
```
echo 'set completion-ignore-case On' | sudo tee -a /etc/inputrc
```

Lightweight prompt with git branch name and status:

```bash
# get current branch in git repo
function parse_git_branch() {
    BRANCH=`git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'`
    if [ ! "${BRANCH}" == "" ]
    then
        STAT=`parse_git_dirty`
        echo "[${BRANCH}${STAT}]"
    else
        echo ""
    fi
}

# change git display language to english first
alias git='LANG=en_GB git'
# get current status of git repo
function parse_git_dirty {
    status=`git status 2>&1 | tee`
    dirty=`echo -n "${status}" 2> /dev/null | grep "modified:" &> /dev/null; echo "$?"`
    untracked=`echo -n "${status}" 2> /dev/null | grep "Untracked files" &> /dev/null; echo "$?"`
    ahead=`echo -n "${status}" 2> /dev/null | grep "Your branch is ahead of" &> /dev/null; echo "$?"`
    newfile=`echo -n "${status}" 2> /dev/null | grep "new file:" &> /dev/null; echo "$?"`
    renamed=`echo -n "${status}" 2> /dev/null | grep "renamed:" &> /dev/null; echo "$?"`
    deleted=`echo -n "${status}" 2> /dev/null | grep "deleted:" &> /dev/null; echo "$?"`
    bits=''
    if [ "${renamed}" == "0" ]; then
        bits=">${bits}"
    fi
    if [ "${ahead}" == "0" ]; then
        bits="*${bits}"
    fi
    if [ "${newfile}" == "0" ]; then
        bits="+${bits}"
    fi
    if [ "${untracked}" == "0" ]; then
        bits="?${bits}"
    fi
    if [ "${deleted}" == "0" ]; then
        bits="x${bits}"
    fi
    if [ "${dirty}" == "0" ]; then
        bits="!${bits}"
    fi
    if [ ! "${bits}" == "" ]; then
        echo " ${bits}"
    else
        echo ""
    fi
}


export PS1='\[\e[1;36m\] \n$PWD \[\e[1;35m\]`parse_git_branch` \n\[\e[1;37m\]\[\033[38;5;10m\]\\$\[$(tput sgr0)\] '
```


# zsh

1- First install zsh with
```Bash
sudo apt install zsh
```

2- Then install oh-my-zsh

```Bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```


3- (optional) install auto-suggestion plugin:

```Bash
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
```

## .zshrc file

```Bash
# Path to your oh-my-zsh installation.
export ZSH="/home/USERNAME/.oh-my-zsh"
 
ZSH_THEME="avit"

COMPLETION_WAITING_DOTS="true"

# Which plugins would you like to load?
# Standard plugins can be found in ~/.oh-my-zsh/plugins/*
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Add wisely, as too many plugins slow down shell startup.
plugins=(git zsh-autosuggestions)

source $ZSH/oh-my-zsh.sh

cd
```

# vim

## .vimrc file

```Bash
colorscheme slate
colorscheme koehler
set number 
syntax on 

set encoding=utf-8
set pastetoggle=<F2>

set scrolloff=3

set wrap
set textwidth=79

set tabstop=4
set shiftwidth=4
set softtabstop=4
set expandtab

set ttyfast
```

