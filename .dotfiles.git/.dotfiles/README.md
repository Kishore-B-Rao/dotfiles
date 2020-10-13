
### Brew installaation
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

### iterm2 installation
brew cask install iterm2

### Install zsh
brew install zsh

### Set zsh as your default shell
Add `/usr/local/bin/zsh` to `/etc/shells`. Note you will need to be sudo to do this:
Run `chsh -s /usr/local/bin/zsh`
Run `sudo dscl . -create /Users/$USER UserShell /usr/local/bin/zsh` to make sure the correct version of zsh is being used

### Install Oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

### Install powerlevel10k
`git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`
Add `ZSH_THEME="powerlevel10k/powerlevel10k"` to your `.zshrc`
Config wizard should come up right away if not type `p10k configure`
Wizard should do the following:
1. Install meslo fonts 
You can go through and custmize but syncing the .p10k.configure file will keep it consistent
Note that the install should add the following to your .zshrc
```
# Enable Powerlevel10k instant prompt. Should stay close to the top of ~/.zshrc.
# Initialization code that may require console input (password prompts, [y/n]
# confirmations, etc.) must go above this block; everything else may go below.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi
# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

```

### Install fzf
1. `git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf`
2. `~/.fzf/install`
Note this will also add  `[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh` to your zshrc

### Virtualenv and Virtualenvwrapper
`pip3 install virtualenv virtualenvwrapper` or `pip install virtualenv virtualenvwrapper` for python2 version
If you install with Pip 3 you will need `export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3` in .zshrc
Other configs should be synced via the dofile but here it is to be sure:

```
################# Virtualenv and Virtualenvwrapper Setup ##############
# set where virutal environments will live
export WORKON_HOME=$HOME/.virtualenvs
# use the same directory for virtualenvs as virtualenvwrapper
export PIP_VIRTUALENV_BASE=$WORKON_HOME
# makes pip detect an active virtualenv and install to it
export PIP_RESPECT_VIRTUALENV=true
if [[ -r /usr/local/bin/virtualenvwrapper.sh ]]; then
  source /usr/local/bin/virtualenvwrapper.sh
else
  echo "WARNING: Can't find virtualenvwrapper.sh"
fi

```

### Syncing Dotfiles
Dotfiles synced via https://www.anand-iyer.com/blog/2018/a-simpler-way-to-manage-your-dotfiles.html

#### To create for the first time:

`mkdir $HOME/.dotfiles`
`git init --bare $HOME/.dotfiles`

Add alias to zshrc
`alias dotfiles='/usr/local/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME`

Configure Repository
`dotfiles config --local status.showUntrackedFiles no`
`dotfiles remote add origin git@github.com:Kishore-B-Rao/dotfiles.git`


#### New Machine Setup

Run the following from the root:
1. `git clone --separate-git-dir=$HOME/.dotfiles git@github.com:Kishore-B-Rao/dotfiles.git tmpdotfiles`
2. `rsync -n --recursive --verbose --exclude '.git' tmpdotfiles/ $HOME/` <-- `-n` flag does a dry run so you can see what files will change
3. `rsync --recursive --verbose --exclude '.git' tmpdotfiles/ $HOME/`
4. `rm -r tmpdotfiles`

### Configs that you care about
.zshrc
	.aliases
	.local
.gitconfig
	.local(dont have yet)
.vimrc
.p10k.zsh