# Rendu Flavien CHAGRAS

## 1.2 Compte Github
```bash
git config --global user.name "Flavien Chagras"
git config --global user.email "flavien.chagras@viacesi.fr"
git config --global color.ui auto
```
Mon fichier .gitconfig
```
[user]
        name = Flavien Chagras
        email = flavien.chagras@viacesi.fr
[color]
        ui = auto
```

## 1.3 Premier Dépôt
```bash
git clone git@github.com:Flachag/gitexercices.git
git add README.md
git commit -m "Ajout du README"
git push
```

## 1.4 Ignorer des fichiers
```bash
git add .gitignore
git commit -m "Ajout du gitignore"
git push
```

Le gitignore sert à ignorer une partie des fichiers/arborescence.

On ignore les fichiers compilés, les logs, les packages ou les fichiers en rapport avec les IDE.

Le but est d'éviter de polluer le dépôt avec des fichiers qui sont générés ou avec des données sensibles (le plus souvent dans les fichiers de configuration).
```gitignore

# Created by https://www.toptal.com/developers/gitignore/api/java
# Edit at https://www.toptal.com/developers/gitignore?templates=java

### Java ###
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

# End of https://www.toptal.com/developers/gitignore/api/java
```

## 1.5 Embranchement avec le git-flow : nouvelle fonctionnalité
```bash
git checkout -b dev master
git push --set-upstream origin dev

git checkout -b notificationParEmail dev
git add notification.txt
git commit -m "Ajout du fichier Notification"
git push --set-upstream origin notificationParEmail

git add notification.txt
git commit -m "Modification de notification"
git push

git checkout dev
git merge notificationParEmail
git push origin --delete notificationParEmail
```

## 1.6 Embranchement avec le git-flow : release
```bash
git checkout -b RealeaseCandidate1.2 dev
git add notification.txt
git commit -m "Fix de notification"
git push --set-upstream origin RealeaseCandidate1.2

git checkout master
git merge RealeaseCandidate1.2
git tag -a v1.2 -m "RealeaseCandidate1.2"
git push --tags

git checkout dev
git merge RealeaseCandidate1.2
git push origin --delete RealeaseCandidate1.2
```

## 1.7 Crochets
prepare-commit-msg avec le CESI-[0-9]+
```bash
#!/bin/bash

BRANCH_NAME="$(git rev-parse --abbrev-ref HEAD)"
valid_chars="CESI-[0-9]+"

if [[ ! $BRANCH_NAME =~ $valid_chars ]]
then
    printf '%s\n' "Your name branch must match with this regex CESI-[0-9]+"
    exit 1
fi

echo "[$BRANCH_NAME] $(cat $1)" > $1
exit 0
```

prepare-commit-msg avec le nom de la branche actuelle
```bash
BRANCH_NAME=$(git branch 2>/dev/null | grep -e ^* | tr -d ' *')

if [ -n "$BRANCH_NAME" ] && [ "$BRANCH_NAME" != "master" ] && [ "$BRANCH_NAME" != "(nobranch)" ]; then
    echo "#$BRANCH_NAME $(cat $1)" > $1
fi
```

pre-push
```bash
#!/bin/bash

BRANCH=`$(git rev-parse --abbrev-ref HEAD)`
PROTECTED_BRANCHES="^(master|main)"

if [[ "$BRANCH" =~ $PROTECTED_BRANCHES ]]; then
  read -p "Are you sure you want to push to \"$BRANCH\" ? (y/n): " -n 1 -r < /dev/tty
  echo
  if [[ $REPLY =~ ^[Yy]$ ]]; then
    exit 0
  fi
  echo "Push aborted."
  exit 1
fi
exit 0
```

pre-commit
```bash
if [[ ! $(`java -jar lib/google-java-format-1.10-all-deps.jar –replace src/*.java`) ]]
then
  exit 1
fi

if [[ ! $(`java -jar lib/spotbugs-4.3.0/lib/spotbugs.jar -textui src/*.class`) ]]
then
  exit 1
fi

exit 0
```

git leaks (dans pre-push)
```bash
if [[ $(`gitleaks --repo-url=https://github.com/Flachag/gitexercices -v`) ]]
then
  exit 0
fi
exit 1
```
