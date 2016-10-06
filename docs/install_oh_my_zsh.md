Install oh-my-zsh
=================

1. Install zsh and git:
    ```
$ sudo apt-get install git zsh curl -yq
    ```

2. Install via curl:
    ```
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    ```

3. Set theme of oh-my-zsh to [pure](https://github.com/sindresorhus/pure):
    ```
➜  ~  git clone https://github.com/sindresorhus/pure .pure
➜  ~  sudo ln -s /home/${USER}/pure/pure.zsh /usr/local/share/zsh/site-functions/prompt_pure_setup
➜  ~  sudo ln -s /home/${USER}/.pure/async.zsh /usr/local/share/zsh/site-functions/async
    ```
    Add two lines to `~/.zshrc` and re-login.
    ```
autoload -U promptinit && promptinit
prompt pure
    ```
