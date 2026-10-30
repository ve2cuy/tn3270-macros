# Macros TN3270

Macros pour le terminal TN3270 pour iPad et Mac de VE2CUY : connexion à TSO, travaux,
console de l'opérateur de [ibm-z-emu](https://ve2cuy.github.io/ibm-z-emu/) (MVS 3.8j
TK5). L'application les charge d'ici (panneau Macros, « Charger du dépôt… »).

*TN3270 macros for VE2CUY's TN3270 terminal for iPad and Mac (TSO logon, jobs, operator
console of ibm-z-emu running MVS 3.8j TK5). The app loads them from here (Macros panel,
"Load from the repository…").*

## Contenu

| Fichier | Macro |
|---|---|
| `tk5/tso-logon.mac` | Connexion TSO jusqu'au menu d'ISPF |
| `tk5/tso-logoff.mac` | Sortie d'ISPF et LOGOFF |
| `tk5/submit-job.mac` | SUBMIT d'un JCL, puis STATUS jusqu'à la fin du travail |
| `tk5/console-command.mac` | Commande d'opérateur (`ibmz-relay --console 3271`) |
| `tk5/console-shutdown.mac` | Arrêt propre de TK5 (S SHUTDOWN jusqu'à IEE334I) |

Essayées sur TK5 Update 5 (MVS 3.8j) dans ibm-z-emu, le 2026-10-30.

## Ajouter une macro

1. Le fichier `.mac` dans le dossier du système (`tk5/`, …), en UTF-8.
2. Son entrée dans `index.json` : `id` unique (sert à mettre à jour la macro déjà chargée),
   `file`, `system`, `name` et `description` en anglais (`en`) et en français (`fr`).

## Langage

Une commande par ligne; `#` commence un commentaire; texte entre guillemets (`\"` pour un
guillemet); `{nom}` est remplacé par une valeur demandée. Majuscules et minuscules sont
confondues dans les attentes.

```text
ask user "User ID" "HERC01"     demander une valeur (avec sa valeur par défaut)
secret password "Password"      demander une valeur cachée
wait "READY"                    attendre le texte à l'écran, clavier libre (30 s)
wait 120 "READY" "***"          délai en secondes; l'un ou l'autre texte
type "LOGON {user}"             taper au curseur
at 23 12                        curseur à la ligne 23, colonne 12
enter  clear  pf1…pf24  pa1…pa3
tab  backtab  home  newline  eraseeof  eraseinput  reset
pause 2                         attendre 2 secondes
if "***" enter                  commande faite si le texte est à l'écran
ifnot "READY" break             … s'il n'y est pas
if blank enter                  … si l'écran est vide (ifnot blank : s'il ne l'est pas)
repeat 10 … end                 répéter le bloc (break : en sortir)
stop "Message"                  fin de la macro, avec un message
```

## Licence

Apache 2.0 (`LICENSE`).
