---
type: single
title: "My ZSH Dotfile"
permalink: /my-personal-zsh-dotfiles/
excerpt: "This is my ZSH configuration file, which gives me a custom terminal line where i can load POWERLEVEL9K UI, which makes it possible to get git repo feedback and display wifi strength."
header:
  image: "/assets/images/git-banner.png"
  teaser: "/assets/images/git-banner.png"
author_profile: false
comments: true
draft: false
---

Sharing is caring!

### This is what my command line in I-Term looks like:

<img src="https://i.ibb.co/rGr3dmq/Screenshot-2019-06-19-at-10-10-30.png" alt="Screenshot-2019-06-19-at-10-10-30" border="0"><br />

Left side:
- OS.
- Shortened pw path.
- PW Repository Git status.
- Current time.

Right side: 
- Successfull/Not successfull command tick
- Wifi name and download speed

### Configuration includes the following features:

tmux configuration:

- Improve color resolution.
- Remove administrative debris (session name, hostname, time) in status bar.
- Set prefix to Ctrl+s
- Soften status bar color from harsh green to light gray.

git configuration:

- Adds a create-branch alias to create feature branches.
- Adds a delete-branch alias to delete feature branches.
- Adds a merge-branch alias to merge feature branches into master.
- Adds an up alias to fetch and rebase origin/master into the feature branch. Use git up -i for interactive rebases.
- Adds post-{checkout,commit,merge} hooks to re-index your ctags.
- Adds pre-commit and prepare-commit-msg stubs that delegate to your local config.
- Adds trust-bin alias to append a project's bin/ directory to $PATH.

Ruby configuration:

- Add trusted binstubs to the PATH.
- Load the ASDF version manager.

Shell aliases and scripts:

- b for bundle.
- g with no arguments is git status and with arguments acts like git.
- migrate for rake db:migrate && rake db:rollback && rake db:migrate.
- mcd to make a directory and change into it.
- replace foo bar **/*.rb to find and replace within a given list of files.
- tat to attach to tmux session named the same as the current directory.
- v for $VISUAL.


## My .zshrc file

```
##########
## TERM ##
##########

export TERM="xterm-256color"

##################
## ZSH SETTINGS ##
##################

ZSH_THEME="powerlevel9k/powerlevel9k"
COMPLETION_WAITING_DOTS="true"

plugins=(git colored-man-pages colorize github ruby rails brew osx zsh-syntax-highlighting)

alias zshconfig="mate ~/.zshrc"
alias ohmyzsh="mate ~/.oh-my-zsh"

export UPDATE_ZSH_DAYS=13
export ZSH=$HOME/.oh-my-zsh

source $ZSH/oh-my-zsh.sh

if [ -f ~/.config/exercism/exercism_completion.zsh ]; then
  . ~/.config/exercism/exercism_completion.zsh
fi

################################
# GET INTERNET SIGNAL IN Mb/s ##
################################

zsh_wifi_signal(){
        local output=$(/System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport -I)
        local airport=$(echo $output | grep 'AirPort' | awk -F': ' '{print $2}')

        if [ "$airport" = "Off" ]; then
                local color='%F{yellow}'
                echo -n "%{$color%}Wifi Off"
        else
                local ssid=$(echo $output | grep ' SSID' | awk -F': ' '{print $2}')
                local speed=$(echo $output | grep 'lastTxRate' | awk -F': ' '{print $2}')
                local color='%F{yellow}'

                [[ $speed -gt 100 ]] && color='%F{green}'
                [[ $speed -lt 50 ]] && color='%F{red}'

                echo -n "%{$color%}$ssid $speed Mb/s%{%f%}" # removed char not in my PowerLine font
        fi
}

###########################
## POWERLEVEL9K SETTINGS ##
###########################

POWERLEVEL9K_MODE='nerdfont-complete'

### WI-FI settings
POWERLEVEL9K_CUSTOM_WIFI_SIGNAL="zsh_wifi_signal"
POWERLEVEL9K_CUSTOM_WIFI_SIGNAL_BACKGROUND="black"


#### Elements
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(root_indicator os_icon dir vcs virtualenv )
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=( status custom_wifi_signal)

### Other
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_STATUS_OK_IN_NON_VERBOSE=true
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_RPROMPT_ON_NEWLINE=true
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX=''
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%K{white}%F{black} `date +%T` %f%k%F{black}%f "
POWERLEVEL9K_SHORTEN_DIR_LENGTH=3

#### Colors
POWERLEVEL9K_DIR_HOME_FOREGROUND="white"
POWERLEVEL9K_DIR_HOME_SUBFOLDER_FOREGROUND="white"
POWERLEVEL9K_DIR_DEFAULT_FOREGROUND="white"
POWERLEVEL9K_OS_ICON_BACKGROUND="white"
POWERLEVEL9K_OS_ICON_FOREGROUND="green"

#########################
# Terminal keybindings ##
#########################

bindkey "^[^[[D" backward-word
bindkey "^[^[[C" forward-word
bindkey "^[a" beginning-of-line
bindkey "^[e" end-of-line

###########
## PATHS ##
###########

export PATH=$HOME/bin:/usr/local/bin:$PATH
export PATH="$PATH:$HOME/.rbenv/bin:/usr/local/opt/phantomjs/bin"
export PATH="$PATH:$HOME/.npm-global/bin"
export PATH="$PATH:/usr/local/opt/elasticsearch@2.4/bin"
export PATH="$PATH:/usr/local/opt/postgresql@10/bin"
export PATH="$PATH:/opt/metasploit-framework/bin"

###############
## LANGUGAGE ##
###############

export LANGUAGE=en_US.UTF-8
export LANG=en_US.UTF-8
export LC_CTYPE=UTF-8
export LC_ALL=en_US.UTF-8

#########
## GPG ##
#########

export GPG_TTY=$(tty)

#########
## NVM ##
#########

export NVM_DIR="/Users/horse/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

############
## SPRING ##
############

export DISABLE_SPRING=true

eval "$(rbenv init -)"
eval "$(direnv hook zsh)"

##################
## GoogleCloud  ##
##################

#### The next line updates PATH for the Google Cloud SDK.
if [ -f '/Users/horse/Downloads/google-cloud-sdk/path.zsh.inc' ]; then . '/Users/horse/Downloads/google-cloud-sdk/path.zsh.inc'; fi

#### The next line enables shell command completion for gcloud.
if [ -f '/Users/horse/Downloads/google-cloud-sdk/completion.zsh.inc' ]; then . '/Users/horse/Downloads/google-cloud-sdk/completion.zsh.inc'; fi
```

### Learn more about customizing your terminal line by checking out the [powerlevel9k's repository](https://github.com/Powerlevel9k/powerlevel9k)!
