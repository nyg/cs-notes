# Qt Creator on macOS

## Install

```sh
$ brew install cmake qt
$ brew cask install qt-creator
```

## Config

If Kits, Qt Versions and CMake are not autodetected, in `Qt Creator > Preferences > Kits`:

1. Add Qt Version (path to `qmake`)

    ```sh
    # because Qt Creator won't let you choose binary from /usr
    ln -s /usr/local/opt/qt/bin/qmake ~/Desktop
    # add Qt Version, quit Qt Creator and then do
    atom .config/QtProject/qtcreator/qtversion.xml
    rm ~/Desktop/qmake
    ```

2. Add CMake with path to `cmake`.
3. Add Kit with default values.
