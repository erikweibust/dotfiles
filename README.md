# dotfiles
Repo to store my dotfiles

Finally built a dotfiles repo. Have rolled my bashrc and aliases file into this repo.

The bashrc gets symlinked into my $HOME and then loads all the files in this repo.
Note:
This will only work if your username is erik. If not, you need to replace all instances of erik with your username.

To use:
1. clone this repo to $HOME
2. mv dotfile .dotfiles
3. run ~/.dotfiles/install

### Note

This repo will not overwrite an existing .vimrc or .bashrc. If you have system config currently defined in those files you should move it to the dotfiles corresponding files (i.e., `bash/bashrc` or `system/*`)

**To consider or improve**
This only works if there is a `~/.profile` or `~/.bash_profile` that loads .bashrc

## TODO:
* bash_profile
* ~~vimrc~~
* ssh
* gitconfig

I'm sure there is more but I'll feel better when crossing the above off. Check [my Issues](https://github.com/erikweibust/dotfiles/issues) for what I want to do and the improvements I've already made.

## Credits
I followed along with the following two blogs
1. (http://iamnotmyself.com/2020/11/10/your-terminal-and-you-dotfiles/)
2. (https://medium.com/@webprolific/getting-started-with-dotfiles-43c3602fd789)
