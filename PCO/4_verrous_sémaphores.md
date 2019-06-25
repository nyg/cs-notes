# Verrous et Sémaphores

## Verrous

* Les **verrous** permettent de se passer de l'attente active qui a le désavantage de monopoliser le processeur. Un verrou est une variable booléenne qui possède une liste d'attente ainsi que deux opérations atomiques : vérouiller et déverrouiller. **Mutex** en anglais.
* Le thread verrouillant le verrou est son **propriétaire** jusqu'à ce qu'il le déverrouille (et seul le propriétaire peut le déverrouiller).
* À sa création, un verrou est déverrouillé.

```cpp
void verrouiller(verrou v) {
    if (v)
        v = false
    else
        suspendre la tâche appelante
        et la mettre dans la file d'attente de v
}

void déverrouiller(verrou v) {
    if (la file d'attente != vide)
        débloquer une tâche de la file d'attente
    else
        v = true
}
```

### API Qt

```cpp
#include<QMutex>
QMutex(RecursionMode mode = NonRecursive)
QMutex::lock()
QMutex::unlock()
QMutex::tryLock(int timeout = 0)
```

### Sémaphores

* Les **sémaphores** sont une généralisation des verrous. Ils comprennent une variable entière au lieu d'un booléen. Les deux opérations atomiques sont P pour l'acquérir et V pour le relacher.
* Un verrou est un sémaphore initialisé à 1.
* Lorsque plusieurs tâches peuvent accéder en même temps à une section critique on parle alors de **section contrôlée**.
* Les sémaphores servent à coordonner ou synchroniser des tâches (i.e. faire en sorte qu'une tâche ne s'exécute que lorsqu'une ou plusieurs autres ont terminé). Un exemple consiste à initialiser un sémaphore à 0, ainsi, le premier appel à P sera bloquant.

```cpp
void P(sémaphore s) {
    s -= 1
    if (s < 0)
        suspendre la tâche appelante
        et la mettre dans la file d'attente de s
    }
}

void V(sémaphore s) {
    s += 1;
    if (s <= 0)
        débloquer une tâche de la file d'attente
    }
}
```

### API Qt

```cpp
#include<QSemaphore>
QSemaphore(int n = 0)
QSemaphore::acquire(int n = 1)
QSemaphore::release(int n = 1)

// optionnelles
QSemaphore::tryAcquire(int n = 1)
QSemaphore::available()
```
