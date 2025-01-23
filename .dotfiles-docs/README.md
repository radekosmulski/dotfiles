# dotfiles

A bare Git repository for dotfiles provides a clean way to track configuration files in your home directory without needing a separate working directory. This approach allows you to maintain version control of your dotfiles while keeping them in their original locations, avoiding symlinks or copying files between a repository and your home directory.

## Create bare repo in your `$HOME`

```
# Create a bare repo
git init --bare $HOME/.dotfiles

# Create an alias for managing dotfiles
alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'

# Add the alias to your .zshrc/.bashrc
echo "alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> .zshrc

# Hide untracked files
dotfiles config --local status.showUntrackedFiles no
```

You can then add your dotfiles like so:

```
dotfiles status
dotfiles add .vimrc
dotfiles commit -m "Add vimrc"
dotfiles push
```

## Check out the repo on a new machine


```
# Clone the bare repo
git clone --bare git@github.com:radekosmulski/dotfiles.git $HOME/.dotfiles

# Create the alias
alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'

# Checkout the files (might fail if you have existing configs)
dotfiles checkout

# Hide untracked files
dotfiles config --local status.showUntrackedFiles no
```

If the checkout fails because you already have files with the same names in your `$HOME` as commited in the repo, first backup these files:

```
mkdir -p .config-backup && \
dotfiles checkout 2>&1 | grep -E "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} .config-backup/{}
```

And retry checkout:
```
dotfiles checkout
```

Don't forget to add the alias to your .zshrc/.bashrc on the new machine too:
```
echo "alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> ~/.zshrc
```

## Creating a README.md

```
mkdir ~/.dotfiles-docs
vim ~/.dotfiles-docs/README.md
dotfiles add .dotfiles-docs/README.md
dotfiles commit -m "Add README"
```

## Switching to `zsh` on ubuntu server

```
sudo apt install zsh
chsh -s $(which zsh)

# install oh-my-zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
