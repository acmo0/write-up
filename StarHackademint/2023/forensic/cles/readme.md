# J'ai perdu mes clés
## Énnoncé
Bon, je sais bien qu'on avait prévu un apéro chez moi ce soir, mais j'ai perdu mes clés. Elles doivent bien être quelque part dans ma mémoire, tu penses pouvoir m'aider ? Ce serait quand même dommage de devoir annuler.

Analysez le vidage mémoire de votre connaissance et servez vous de vos trouvailles pour vous rendre à la fête.

## Solution
On a à notre dispotion un dump de la RAM d'une machine.
On cherche des mots-clés contenus dans le fichier à l'aide de `strings` :
```bash
strings dump.elf |grep 'KEY'
```
En fouillant un peu on trouve des en-têtes de clés ssh
On retouve différentes clés ssh en exécutant :
```bash
strings dump.elf | grep -A 60 '-----BEGIN OPENSSH PRIVATE KEY-----'
```
Il faut maintenant essayer de trouver le nom d'utilisateur, le host et le port pour se connecter en ssh.
En utilisant volatility :
```bash
vol --profile=Win7SP1x64 -f dump.elf cmdscan

```
Output :
```
**************************************************
CommandProcess: conhost.exe Pid: 2808
CommandHistory: 0x290070 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 11 LastAdded: 10 LastDisplayed: 10
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x10
Cmd #0 @ 0x2641d0: cd OpenSSH-Win64\OpenSSH-Win64
Cmd #1 @ 0x28cd10: ssh-keygen.exe
Cmd #2 @ 0x28cda0: cd ..\..\.ssh\
Cmd #3 @ 0x28c5f0: dir
Cmd #4 @ 0x289250: type id_rsa
Cmd #5 @ 0x28cdd0: type id_rsa.pub
Cmd #6 @ 0x2891b0: cd ..\..\
Cmd #7 @ 0x289230: cd John
Cmd #8 @ 0x264220: cd OpenSSH-Win64\OpenSSH-Win64
Cmd #9 @ 0x28c600: dir
Cmd #10 @ 0x299720: ssh -i ..\..\.ssh\id_rsa user@challenges.hackademint.org -p 30997
Cmd #15 @ 0x1f0158: )
Cmd #16 @ 0x1f0158: )
```
On se connectera en exécutant :
```
ssh -i <nom de la clé> user@challenges.hackademint.org -p 30997
```
On sépare les différentes clés ssh et en les essayant successivement on fini par réussir à s'y connecter et obtenir le flag !