## Installation (creating a bare repo to track the dotfiles)

```
# Create a bare repo
git init --bare $HOME/.dotfiles
# Create an alias for managing dotfiles
alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
# Add the alias to your .zshrc/.bashrc
echo "alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> .zshrc
# Hide untracked files
source ~/.zshrc
dotfiles config --local status.showUntrackedFiles no
```
