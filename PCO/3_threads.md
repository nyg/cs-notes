# Threads

* **Green thread** : thread ordonnancé par une machine virtuelle ou une librairie qui émule l'ordonnanceur de l'OS.
* **Native thread** : thread natif géré par l'ordonnanceur de l'OS.

## QThread

```cpp
class MyThread : public QThread {

    private:
        virtual void run() Q_DECL_OVERRIDE {
            // code du thread
        }
};
```

```cpp
int main(int argc, char *argv[]) {
    MyThread thread;
    thread.start(); // lance l’exécution du thread thread.wait();  // attend la terminaison du thread return 0;
}
```

Un thread se termine lorsqu'il exécute l'instruction `return`. Depuis un autre thread il peut être interrompu avec :

```cpp
QThread::terminate() // à eviter

QThread::requestInterruption()
QThread::isInterruptionRequested() // depuis le thread concerné
```

```cpp
QThread::currentThreadId()    // retourne l'id the thread courrant
QThread::yieldCurrentThread() // le thread appelant est préempté
```
