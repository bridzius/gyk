# [Grow Your Knowledge] - git intro

`--help` pridėjimas prie bet kurios komandos atidaro dokumentaciją apie tą komandą.

## Sukūrimas

### git init

Sukuria naują vietinę repozitoriją direktorijoje.
Naudojimas:

- `git init`

### git clone

Atsiunčia ir sukuria nuotolinę repozitoriją.
Naudojimas:

- `git clone <repozitorijos-url>`

## Failų operacijos

### git add

Prideda failą į "staging area". Informuoja git, kad reikia track'inti failą.
Naudojimas:

- `git add <failas>` - prideda failą į staging area.
- `git add .` - prideda visus pakeistus/sukurtus failus į staging area.

### git commit

Sukuria naują pakeitimą iš failų, esančių "staging area".
Naudojimas:

- `git commit` - atidaro standartinį tekstinį editorių pridėti žinutei.
- `git commit -m <žinutė>` - commit iš karto su žinute.
- `git commit -a -m <žinutė>` - commit, pridedant visus "žinomus" failus ir žinutę.

### git checkout

Pakeičia branch į kitą arba sukuria naują.
Naudojimas:

- `git checkout <branch>` - pakeičia branch į nurodytą.
- `git checkout -b <branch>` - sukuria naują branch.

### git reset

Ištrina failų pakitimus nuo paskutinio commit. Naudojimas:

- `git reset --hard <commit>`- ištrina failų pakitimus ir commit iki nurodyto commit.
- `git reset --soft <commit>`- ištrina commit iki nurodyto commit, bet palieka failų pakitimus direktorijoje (reikės iš naujo daryti `git add`, kad juos pridėtų).

## Pakeitimų stebėjimas

### git status

Parodo pakeistus ir pridėtus failus. Taip pat nurodo, kurie failai yra "staging area", kurie ne, o kurie vis dar "nematomi" git (untracked).
Naudojimas:

- `git status`

### git diff

Parodo failų pakitimus nuo paskutinio commit.
Naudojimas:

- `git diff` - parodo pakitimus failuose, kurie nėra staging area.
- `git diff --cached` - parodo pakitimus failuose, kurie yra staging area.

### git log

Parodo commitų istoriją.
Naudojimas:

- `git log`

## Repozitorijos sinchronizavimas

### git pull

Parsiunčia repozitorijos atnaujinimus iš serverio (remote) ir iš karto prideda naujus failus. Naudojimas:

- `git pull`

### git fetch

Parsiunčia repozitorijos atnaujinimus, bet direktorijos turinio nekeičia (pakeitimui reikia `git pull`). Naudojimas:

- `git fetch`

### git push

Nusiunčia atnaujinimus (commit'us) iš lokalios repozitorijos į serverį. Naudojimas

- `git push`

Toliau:
`git rebase` ir `git merge`
