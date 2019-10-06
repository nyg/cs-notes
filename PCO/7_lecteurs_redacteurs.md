# 7 – Lecteurs–rédacteurs

* Utilisé lorsque plusieurs threads doivent accéder à une ressource.
* Les lecteurs ne peuvent que lire et les rédacteurs qu'écrire.
* Plusieurs lecteurs peuvent lire en même temps, mais les rédacteurs s'excluent mutuellement.

## Priorité aux lecteurs

* Un lecteur peut accéder à la ressource si :
    * le nombre de rédacteurs en cours d’écriture vaut 0.
* Un rédacteur peut accéder à la ressource si :
    * le nombre de rédacteurs en cours d’écriture vaut 0,
    * le nombre de lecteurs en cours de lecture vaut 0 et
    * le nombre de lecteurs en attente de la ressource vaut 0.

* `nbReaders` : nombre de lecteurs.
* `mutexReaders` : protège l'accès à `nbReaders`.
* `writer` : permet aux lecteurs de bloquer les futurs rédacteurs et permet à un rédacteur de bloquer les lecteurs pendant l’écriture.
* `mutexWriters` : permet au rédacteur de bloquer les autres rédacteurs et empêche un rédacteur de brûler la priorité à un lecteur.

```c++
void lockReading() {
    mutexReaders.acquire();
    nbReaders++;
    if (nbReaders == 1) {
        writer.acquire();
    }
    mutexReaders.release();
}

void unlockReading() {
    mutexReaders.acquire();
    nbReaders--;
    if (nbReaders == 0) {
        writer.release();
    }
    mutexReaders.release();
}

void lockWriting() {
    mutexWriters.acquire();
    writer.acquire();
}

void unlockWriting() {
    writer.release();
    mutexWriters.release();
}
```

## Priorité aux lecteurs si lecture en cours

Idem mais sans `mutexWriters`. Un lecteur ne peut donc pas brûler la priorité à un lecteur.

## Priorité égale

* `fifo` : file d’attente dans laquelle passent tous les lecteurs et rédacteurs.

```c++
void lockReading() {
    fifo.acquire();
    mutex.acquire();
    nbReaders++;
    if (nbReaders == 1) {
        writer.acquire();
    }
    mutex.release();
    fifo.release();
}

void unlockReading() {
    mutex.acquire();
    nbReaders--;
    if (nbReaders == 0) {
        writer.release();
    }
    mutex.release();
}

void lockWriting() {
    fifo.acquire();
    writer.acquire();
}

void unlockWriting() {
    writer.release();
    fifo.release();
}
```

## Priorité aux rédacteurs

* `mutexReaders` : permet de favoriser les rédacteurs en empêchant plusieurs lecteurs de s'ajouter à `reader`.
* `mutexWriters` : qui permet de bloquer les rédacteurs pendant que des écritures ou des lectures sont en cours.
* `mutex` : qui est en charge de protéger l’accès à la variable `nbReaders`.
* `writer` : qui permet au premier lecteur qui accède la ressource de bloquer les potentiels rédacteurs.
* `reader` : qui permet au premier rédacteur arrivé de bloquer les potentiels futurs lecteurs.

```c++
void lockReading() {
    mutexReaders.acquire();
    reader.acquire();
    mutex.acquire();
    nbReaders++;
    if (nbReaders == 1) {
        writer.acquire();
    }
    mutex.release();
    reader.release();
    mutexReaders.release();
}

void unlockReading() {
    mutex.acquire();
    nbReaders--;
    if (nbReaders==0)
        writer.release();
    mutex.release();
}

void lockWriting() {
    mutexWriters.acquire();
    nbWriters++;
    if (nbWriters == 1)
        reader.acquire();
    mutexWriters.release();
    writer.acquire();
}

void unlockWriting() {
    writer.release();
    mutexWriters.acquire();
    nbWriters--;
    if (nbWriters == 0)
        reader.release();
    mutexWriters.release();
}
```
