# Carnet de bord

Identifiant GitHub : addous-ilyes

## Mes preuves

### TP 1 : historique et index
- Commits : `docs: presenter le forum ENSUP`, `docs: preciser les conditions d acces`, `chore: ignorer les fichiers locaux`.
- **Que fait `add` ?** Enregistrer dans l'éditeur modifie seulement le fichier sur le disque. `git add` copie l'état actuel du fichier dans l'index (zone de préparation) : c'est ce contenu-là, et pas le fichier du disque, qui entrera dans le prochain commit.
- **Piège de l'index :** après `add` puis une nouvelle modification sans refaire `add`, `git diff --cached` montre la 1re phrase et `git diff` la 2e. Un commit à ce moment n'aurait contenu que « Entrée gratuite. ».
- `.env` et `notes-privees.txt` sont ignorés (`git check-ignore -v .env` → `.gitignore:1:.env`) et absents de `git ls-files`.
- Correction : mon `.gitignore` avait été créé par erreur comme un dossier ; je l'ai remplacé par un fichier.

### Enquête 1 : le fichier disparu du diff
`git diff` compare le fichier de travail à l'index. Un fichier non suivi (`??` dans `git status --short`) n'est pas dans l'index, donc il n'apparaît pas. Après `git add acces.md`, il apparaît dans `git diff --cached`. Pour retrouver son commit : `git log --oneline -- acces.md` puis `git show <id>`.

### TP 2 : branche feature/programme
`programme.md` n'existait pas sur `main` avant la fusion. Pour intégrer, il faut être sur `main` puis lancer `git merge feature/programme`. `ls` et `git log --graph --oneline --all` le prouvent.

### Enquête 2 : divergence sans conflit
`git merge --no-ff feature/accessibilite` → commit `b246376`, parents `65d51b2` et `7100a3b`. Il y a deux parents et aucun conflit, car les fichiers modifiés étaient différents.

### TP 5 : laboratoire des erreurs
- `git restore --staged brouillon.txt` retire le fichier de l'index sans le supprimer.
- `git revert` crée un nouveau commit inverse : le commit d'ajout reste dans l'historique, et le fichier disparaît.
- Conflit : `feature/horaire` propose 10h00, `main` (HEAD) annonce 09h30. Git ne choisit pas seul, car les deux modifient la même ligne : c'est une décision humaine. Décision du commanditaire : **09h45**.
- Fin de fusion : `git add horaire.txt` puis `git commit -m "fix: convenir de 09h45"`. Commit de fusion `f8057fb`, parents `9459a37` (main) et `598f4e0` (feature/horaire).
- `git merge --abort` permet d'annuler une fusion en cours et de revenir à l'état d'avant.

### Réponses du carnet
- **Que copie add ?** L'état actuel du fichier, vers l'index.
- **Comment prouver qu'un commit a été poussé ?** `git log --oneline origin/main` le contient, et il est visible sur GitHub dans l'onglet Commits.
- **Pourquoi Git n'a-t-il pas choisi l'horaire ?** Les deux branches modifiaient la même ligne : Git ne peut pas deviner l'intention.
- **Que reste-t-il après revert ?** Le commit d'origine et un nouveau commit qui l'annule. L'historique n'est pas effacé.

### TP 3 : collecte
- URL PR de collecte : _à compléter_
- Fork = copie du dépôt sur mon compte GitHub ; clone = copie de ce dépôt sur mon PC ; branche = ligne de travail à l'intérieur d'un dépôt.

### Enquête 3 : push refusé
1. `Permission denied` → `git remote -v` (origin doit viser mon fork).
2. `non-fast-forward` → `git fetch origin` puis `git log --graph --oneline --all`.
3. `nothing to commit` → `git status --short` et `git check-ignore -v FICHIER`.
