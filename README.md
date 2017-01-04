Installation de Powerline Shell
========================

```bash
$ sudo apt-get install python-pip
$ pip install --user git+git://github.com/Lokaltog/powerline
```

Téléchargeons les polices et installons les dans le répertoire de polices d’Ubuntu :

```bash
$ wget https://github.com/Lokaltog/powerline/raw/develop/font/PowerlineSymbols.otf
$ sudo mv PowerlineSymbols.otf /usr/share/fonts/
$ wget https://github.com/Lokaltog/powerline/raw/develop/font/10-powerline-symbols.conf
$ sudo mv 10-powerline-symbols.conf /etc/fonts/conf.d/
$ sudo fc-cache -vf
$ git clone https://github.com/Lokaltog/powerline-fonts.git
$ sudo mv powerline-fonts/ /usr/share/fonts/
$ sudo fc-cache -vf
```

Téléchargeons désormais powerline-shell :

```bash
$ git clone https://github.com/milkbikis/powerline-shell
$ cd powerline-shell
$ cp config.py.dist config.py
$ python install.py
$ cp powerline-shell.py ~
```

Il ne reste plus qu’à modifier votre prompt en ajoutant ces quelques à la fin de votre fichier . bashrc

```bash
$ sudo gedit ~/.bashrc
```

Ajouter en fin du fichier:

```bash
function _update_ps1() {
    PS1="$(~/powerline-shell.py $? 2> /dev/null)"
}

if [ "$TERM" != "linux" ]; then
    PROMPT_COMMAND="_update_ps1; $PROMPT_COMMAND"
fi
```

Installation de ZSH
========================

```bash
$ sudo apt-get install zsh
```

Il faut ensuite paramétrer Zsh comme shell par défaut. Pour cela:

```bash
$ chsh
```

Saisir **/bin/zsh**. Quitter la session et revenir en lançant un terminal. Au premier lancement une configuration par défaut est proposée.

Ensuite, dans le fichier **~/.zshrc**, pour activer powerline:

```bash
function powerline_precmd() {
    PS1="$(~/powerline-shell.py $? --shell zsh 2> /dev/null)"
}

function install_powerline_precmd() {
  for s in "${precmd_functions[@]}"; do
    if [ "$s" = "powerline_precmd" ]; then
      return
    fi
  done
  precmd_functions+=(powerline_precmd)
}

if [ "$TERM" != "linux" ]; then
    install_powerline_precmd
fi
```

Pour une meilleure gestion de la complétion de ZSH, avec notamment la sélection sur tabalution:

```bash
zstyle ':completion:*:descriptions' format '%U%B%d%b%u'
zstyle ':completion:*:warnings' format '%BDésolé, pas de résultats pour : %d%b'
zstyle ':completion:*' menu select=2
zstyle ':completion:*' select-prompt %SScrolling active: current selection at %p%s
```

Pour éviter de proposer un élément déjà présent lors d'un cp, mv ou rm :

```bash
zstyle ':completion:*:rm:*' ignore-line yes
zstyle ':completion:*:mv:*' ignore-line yes
zstyle ':completion:*:cp:*' ignore-line yes
```

Alias
--------
Quelques aliases à ajouter dans le **zshrc**:

```bash
alias gl='git log --graph --decorate=short --color --format=format:"%C(yellow)%h%C(reset) %C(auto)%C(reset)     %C(green)[%cd]%C(reset)  %x09%C(white)%<(60,trunc)%s %C(bold blue)<%an>%C(auto)%d" -n 20'
alias gst='git status'
alias ll='ls -alF'
alias ls='ls --color=auto'
...
```