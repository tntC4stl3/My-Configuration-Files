Install Oracle JDK
=============

## Unbuntu 16.04

1. Add Oracle's PPA and update package repository.
    ```
    ➜  sudo add-apt-repository ppa:webupd8team/java
    ➜  sudo apt-get update
    ```

2. Install Oracle JDK 8
    ```
    ➜  sudo apt-get install oracle-java8-installer
    ```

## Managing Java

If you have multiple Java installation on one server. You can configure which version is the default for use in the command line by using `update-alternatives`:
```
➜  sudo update-alternatives --config java
```
