## iTerm-2 SSH Color Scheme

If you want to change your [iTerm2](http://iterm2.com/) [ZSH](https://github.com/robbyrussell/oh-my-zsh) shell's theme/color scheme when you SSH into a server, follow these directions below.

Create 2 Profiles:

	*	Your personal preference theme (can be named anything)
	*	SSH color theme (must be named SSH)

Go into your `~/.oh-my-zsh/custom` directory and create a new file entitled `iTerm2-ssh.zsh`. Copy and paste the following or copy from the repo:

```bash
function tabc() {
  NAME=$1; if [ -z "$NAME" ]; then NAME="Default"; fi 
  # if you have trouble with this, change
  # "Default" to the name of your default theme
  echo -e "\033]50;SetProfile=$NAME\a"
}

function tab-reset() {
    NAME="YOUR_CUSTOM_PROFILE_NAME_HERE"
    echo -e "\033]50;SetProfile=$NAME\a"
}

function colorssh() {
    if [[ -n "$ITERM_SESSION_ID" ]]; then
        trap "tab-reset" INT EXIT
        if [[ "$*" =~ "web*|production|ec2-.*compute-1" ]]; then
            tabc SSH
        fi
    fi
    ssh $*
}
compdef _ssh tabc=ssh

alias ssh="colorssh"
```

The breakdown of this code:

`function tabc()` grabs the _ssh_ name after the command `$ ssh` is entered. This
changes the _SetProfile_ name to _ssh_.

`tab-reset()` is responsible for when you exit the ssh session to return back to
a profile name of your choosing. *Remember to create a custom profile name and replace
the _YOUR_CUSTOM_PROFILE_NAME_HERE_ with your profile name*.

`colorssh` determines when to change the _SetProfile_ name. Currently it will
changes to the _ssh_ profile when the following values exist after `$ ssh`.

	-	web *
	-	production
	-	ec2-.*compute-1

Go [here](http://iterm2colorschemes.com/) for a library of iTerm2 color themes to choose from.

#### Make sure to start a new tab to see the results.