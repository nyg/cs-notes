# 6 – Moniteurs

* Abstraction de plus haut niveau que les sémaphores.
* La synchronisation s'effectue par des variables de conditions.
* Une variable de condition permet de suspendre le thread (`wait`) ou d'en réveiller un autre (`wake`, `signal`).
* Ces procédures d'attente et de signalisation sont *thread-safe*.

## Attente (`wait`)

* Bloque inconditionnellement le thread appelant.
* Lui fait relâcher l’exclusion mutuelle sur le moniteur.
* Le place dans une file associée à la variable de condition.

## Signalisation (`wake`, `signal`)

* Dépend de l’état de la  file associée à la variable de condition.
* Si elle est vide, le thread appelant poursuit son exécution, l’opération n’a aucun e ffet.
* Si elle n'est pas vide, un des threads bloqués est réactivé et reprend immédiatement son exécution.

## Type de moniteurs

* **Hoare** : le thread réveillé par le signal prend possession du mutex.
* **Mesa** : le thread qui envoie le signal garde le mutex (e.g. Qt, pthread, Java).

## Utilisation avec Qt

* `QMutex` pour assurer l'exclusion mutuelle.
* `QWaitCondition` pour la variable de condition.

### Attente

```c++
bool QWaitCondition::wait(QMutex * lockedMutex,
                          unsigned long time   ULONG_MAX)
```

* La méthode relâche le mutex et suspend l’exécution du thread jusqu’à ce que la variable condition soit signalée.
* Le mutex doit être verrouillé par le thread avant l’appel à `wait`.
* Au moment où la condition est signalée, `wait` re-verrouille automatiquement le mutex.
* **Note** : lors du re-verrouillage le thread est en compétition avec tous les threads demandant le verrou.
 
### Signalisation

```c++
void QWaitCondition::wakeOne();
void QWaitCondition::wakeAll();
```

* Réveille soit un soit tous les threads en attente sur la variable de condition.

### Exemple

```c++
void MyMonitor::oneFunction() {

    mutex.lock();
    while (!uneCondition) {
        cond.wait(&mutex);
    }

    // ...

    mutex.unlock();
}

void MyMonitor::anotherFunction() {

    mutex.lock();

    // ...

    uneCondition = true;
    cond.wakeOne();
    mutex.unlock();
}
```
