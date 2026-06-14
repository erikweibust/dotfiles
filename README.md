## 06/14/26 Update — How this repo actually works

> Notes added while getting two Macs back in sync (and fixing a Ghostty cursor quirk).
> The original README content is below this section — I'll reconcile/prune it later.

### The core idea: symlinks

The real files live in this repo at `~/.dotfiles`, but the tools that read them (bash,
vim, git, Ghostty) expect them at fixed spots in `$HOME`. So `~/.bashrc`, `~/.vimrc`,
etc. are just **symlinks** pointing back into this repo. Edit a file here once, commit,
and every machine that pulls gets the change.

### What happens when I open a terminal

A terminal starts a *login* shell, so macOS reads `~/.bash_profile` first:

1. **`bash/bash_profile`** does one real thing: `source ~/.bashrc`.
2. **`bash/bashrc`** is the engine: runs `brew shellenv`, loops over every file in
   `system/` and sources it, then sources everything in `~/.dotfiles/.secret/` if that
   folder exists, and finally initializes SDKMAN (must be last).

Every file echoes a breadcrumb to `~/.startup.txt` so I can trace load order.

### File map

- **`system/`** (all sourced by bashrc):
  - `env` — `set -o vi`, history sizes, archey banner, `BAT_CONFIG_PATH`
  - `path` — PATH additions (GNU coreutils/sed/grep, Kafka, `~/bin`, …)
  - `aliases` — shortcuts incl. `cat`→`bat` and the `g*` git aliases
  - `functions` — `j8`/`j10`, `mk`, `ppath`, `compareBranch2Remote`
  - `prompt` — `eval "$(starship init bash)"`
  - (loaded in `find` order, so no file may depend on another running first)
- **`config/`** — `vimrc`, `gitconfig`, `gitignore-global`, `ghostty` are symlinked by
  `install`; `bat.conf` is *not* symlinked — `system/env` points `BAT_CONFIG_PATH` at it.
- **`.secret/`** — gitignored, but sourced by bashrc. Machine-local stuff lives here:
  `api_keys`, and `local.sh` for per-machine PATH lines (installer additions) that I
  don't want in the shared repo.
- **`Brewfile`** — manifest of Homebrew formulae, casks, and VS Code extensions.
  `brew bundle install` installs the toolchain; `brew bundle dump --force` refreshes it.

### Keeping it in sync across machines — IMPORTANT

**There is nothing automatic. Sync is manual git.** The Macs drift apart unless I push
from one and pull on the other.

- After changing anything here:
  ```sh
  git -C ~/.dotfiles add -A
  git -C ~/.dotfiles commit -m "describe the change"
  git -C ~/.dotfiles push
  ```
- When I sit down at the other machine:
  ```sh
  git -C ~/.dotfiles pull
  ```
- Check if a machine is behind *without* pulling:
  ```sh
  cd ~/.dotfiles && compareBranch2Remote   # "up to date" / "not up to date"
  ```

After a `pull` that changed PATH/aliases/etc., open a new terminal (or `source
~/.bashrc`) so it takes effect.

### `install` notes

`install` only creates a symlink if the target doesn't already exist, so it won't
clobber existing config — the one exception is `.bash_profile`, which it backs up and
replaces (installers like Homebrew/Python tend to create their own).

---
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
