# 5 – Producteurs-consommateurs

* Un thread **producteur** et un thread **consommateur** communiquent un utilisant un tampon.
* Seul le producteur écrit et seul le consommateur lit dans le tampon.
* Les éléments du tampon ne sont consommés qu'une fois et le sont dans leur ordre de production.
* Si le tampon est plein, le producteur doit attendre. De même, si le tampon est vide le consommateur doit attendre.

```cpp
// producteur
deposer(item) {
    tant que le tampon est plein:
        attendre
    déposer l’item
    signaler ceci au consommateur
}

// consommateur
item prelever() {
    tant que le tampon est vide:
        attendre
    retirer l’item
    signaler ceci au producteur
}
```

## Tampon de taille 1

* On implémente le tampon en utilisant deux sémaphores, **`waitForFull`** qui permet au consommateur d'attendre que le tampon se remplisse, et **`waitForEmpty`** qui permet au producteur d'attendre que le tampon se vide.
* Pour un tampon vide, `waitForEmpty` est initialisée à 1 et `waitForFull` à 0.

```cpp
void put(T item) {
    waitForEmpty.acquire();
    element = item;
    waitForFull.release();
}

T get(void) {
    waitForFull.acquire();
    T item = element;
    waitForEmpty.release();
    return item;
}
```

## Tampon de taille N

* `waitForEmpty` permet d'attendre qu'une place du tampon soit libre.
* `waitForFull` permet d'attendre qu'un élément du tampon puisse être récupéré.

```cpp
void put(T item) {
    waitForEmpty.acquire();
    mutex.acquire(); // section critique
    elements[writePointer] = item;
    writePointer = (writePointer + 1) % bufferSize;
    waitForFull.release();
    mutex.release();
}

T get(void) {
    waitForFull.acquire();
    mutex.acquire(); // section critique
    T item = elements[readPointer];
    readPointer = (readPointer + 1) % bufferSize;
    waitForEmpty.release();
    mutex.release();
    return item;
}
```
