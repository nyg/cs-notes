# Résumé théorie

## Threads

* Les threads se partagent l'espace d'adressage du processus. Ils possèdent leur propre pile et contexte d'exécution (PC + registre). Cycle de vie similaire à celui d'un processus.
* Un **green thread** est un thread ordonnancé par une machine virtuelle ou une librairie qui émule l'ordonnanceur de l'OS.
* Un **thread natif** est géré par l'ordonnanceur de l'OS.

## Verrou vs Sémaphore

* Le propriétaire d'un **verrou** est celui qui le verrouille. Seul le propriétaire peut le déverrouiller.
* Un **sémaphore** peut être acquis et relaché par n'importe quel thread. C'est utile pour coordonner ou synchroniser des tâches (i.e. faire en sorte qu'une tâche ne s'exécute que lorsqu'une ou plusieurs autres ont terminé).

## Hoare vs Mesa

* Avec un moniteur de **Hoare**, le thread signalant perd possession du mutex et le thread signalé s'exécute immédiatement (il prend donc possession du mutex). C'est la raison pour laquelle vérifier la condition avec un `if` est suffisant.
* Avec un moniteur de **Mesa**, le thread signalant garde possession du mutex. Le thread signalé ne s'exécute par forcément tout de suite après le thread signalant, un autre thread peut être exécuté entre les deux. D'où la nécessité du `while` pour vérifier la condition.
* https://pages.mtu.edu/~shene/NSF-3/e-Book/MONITOR/monitor-types.html
