# dotfiles
Repo to store my dotfiles

Finally built a dotfiles repo. Have rolled my bashrc and aliases file into this repo.

The bashrc gets symlinked into my $HOME and then loads all the files in this repo.

To use:
1. clone this repo to $HOME
2. mv dotfile .dotfiles
3. run ~/.dotfiles/install

** to consider or improve:
This only works if there is a .profile that loads .bashrc

## TODO:
* bash_profile
* vimrc
* ssh
* nonorc
* gitconf

I'm sure there is more but I'll feel better when crossing the above off.

## Credits
I followed allong with the following two blogs
1. (http://iamnotmyself.com/2020/11/10/your-terminal-and-you-dotfiles/)
2. (https://medium.com/@webprolific/getting-started-with-dotfiles-43c3602fd789)
