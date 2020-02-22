# jEnv

jEnv is a version manager for Java. It has, however, some limitations:

1. It does not provide the ability to install Java versions, this must be done using `brew`.
2. It will not update the `JAVA_HOME`. The `export` plugin must be enabled, but even then the `JAVA_HOME` will only be updated when the shell (re)starts.

Maybe I should be using [SDKMAN!](https://github.com/sdkman/sdkman-cli)…

## Install jEnv

```sh
# install jenv
brew install jenv

# enable the export plugin
jenv enable-plugin export
```

## Install Java versions

```sh
# for the latest java version
brew cask install adoptopenjdk

# for a specific version
# see https://github.com/AdoptOpenJDK/homebrew-openjdk
brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk8
```

## Configure jEnv

```sh
# inform jenv of the installed java versions
jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-13.0.2.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home

jenv rehash

# verify
jenv versions
```

## Change the Java version

```sh
# set the global version
jenv global 13.0

# set a local per-directory version
jenv local 1.8

# or a per-shell version
jenv shell 1.8
```

## Misc

```sh
# to see if everything is ok, or not
jenv doctor

# list all commands
jenv commands

# help for a specific command
jenv help <command>
```
