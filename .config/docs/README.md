
---

````markdown

---

## ⚙️ Initial Setup (on a New Machine)

Clone the repository as a **bare repo** into `~/.cfg`:

```bash
git clone --bare git@github.com:<your-username>/dotfiles.git $HOME/.cfg
````

> Replace `<your-username>` with your GitHub username.

---

### 1. Define the Alias

Add this line to your shell configuration file (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

Then reload your shell:

```bash
source ~/.bashrc
```

or

```bash
source ~/.zshrc
```

---

### 2. Checkout Your Dotfiles

Run:

```bash
config checkout
```

If you see errors about existing files, back them up first:

```bash
mkdir -p ~/dotfiles-backup
config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} ~/dotfiles-backup/{}
config checkout
```

Then set Git to ignore untracked files in your `$HOME` directory:

```bash
config config --local status.showUntrackedFiles no
```

---

### 3. Done 

Your configuration files are now in place and managed with Git.

You can now use commands like:

```bash
config status
config add .config/nvim
config commit -m "Update Neovim config"
config push
```

---

##  Tips

* **Add new dotfiles:**

  ```bash
  config add .zshrc
  config commit -m "Track zshrc"
  config push
  ```

* **Update existing configs:**

  ```bash
  config pull
  ```

* **List all tracked files:**

  ```bash
  config ls-tree -r main --name-only
  ```

---

##  Optional Short Alias

Add this helper to your shell for convenience:

```bash
alias cf='config'
```

Now you can use short commands like:

```bash
cf add .config/alacritty
cf commit -m "Update Alacritty config"
cf push
```

---

##  Example Repository Structure

```
~/.config/
  ├── nvim/
  ├── alacritty/
  └── starship.toml
.bashrc
.zshrc
.tmux.conf
.gitignore
```

---

## 🛠️ Troubleshooting

If `config checkout` complains about existing files:

* Move them aside or delete them if no longer needed.
* Then rerun `config checkout`.

If you ever lose the alias, re-add it with:

```bash
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

---

Inspired by [Atlassian’s “Manage your dotfiles with Git” guide](https://www.atlassian.com/git/tutorials/dotfiles).

---

