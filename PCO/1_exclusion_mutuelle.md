# Exclusion mutuelle par attente active

* Une **ressource critique** est une ressource qui ne doit être accédée par une seule tâche à l'instant t.
* La portion de code accédant à une ressource critique est appelée **section critique**.
* L'**exclusion mutuelle** doit être assurée dans une section critique, i.e. les accès s'excluent mutuellement.
* L'accès à une section critique s'oppère par un algorithme d'exclusion mutuelle en deux parties : le protocole d'entrée et celui de sortie (de la section critique).
* Lors d'une **attente active** les cycles CPU sont gaspillé pour tester la valeur d’une condition (e.g. boucle while). Le processus n'est alors préempté que par l'ordonnanceur.
* Lors d'une **attente passive** un processus est bloqué par le noyau (mode waiting) et le processeur est disponible pour exécuter un autre processus.

## Algorithme de Dekker

```cpp
void T0::run() {

    while (true) {

        setEnSectionCritique();
        while (autreEnSectionCritique()) {
            if (pasMonTour()) {
                clearEnSectionCritique();
                while(pasMonTour())
                    ;
                setEnSectionCritique();
            }
        }

        /* section critique */
        […]
        passeMonTour();
        clearEnSectionCritique();
        /* section non-critique */
    }
}
```

## Algorithme de Peterson

```cpp
void T0::run() {

    while (true) {

        setIntention();
        setTourAutre();
        while (intentionAutre() && pasMonTour())
            ;

        /* section critique */
        […]
        clearIntention();
        /* section non-critique */
    }
}
```
