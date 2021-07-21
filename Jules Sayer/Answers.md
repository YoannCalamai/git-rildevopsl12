# Réponses pour le TP Git

## Jules Sayer - RIL DEVOPS L12


### Question 3
https://github.com/Wilders/gitexercices
```bash
git clone git@github.com:Wilders/gitexercices.git
git add README.md
git commit -m "Added README.md"
git push
```

### Question 4
```bash
git add .gitignore
git commit -m "Added .gitignore"
git push
```
Utilisation de gitignore.io avec le langage Java pour exclure les fichiers de build...etc

### Question 5
```bash
git branch dev
git checkout dev

git branch notificationParEmail
git add notification.txt
git commit -m "Added notification.txt"
git push

git checkout dev
git merge notificationParEmail
git branch -d notificationParEmail
git push
```

### Question 6
```bash
git branch RealeaseCandidate1.2
git checkout RealeaseCandidate1.2
git commit -m "Fixed bug in notification.txt"
git push
git checkout main
git merge RealeaseCandidate1.2
git tag -a v1.2 -m "RealeaseCandidate1.2"

git checkout dev
git merge RealeaseCandidate1.2
git push
```

### Question 7
Voir les hooks