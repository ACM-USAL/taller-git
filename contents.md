# ¿Qué es git?

Git es un sistema de control de versiones.

[Web](http://git-scm.com)

## Instalar git

### Linux

#### Ubuntu/Debian

```
[sudo] apt-get install git
```

#### Fedora

```
[sudo] yum install git
```

### OSX

#### Con [homebrew](http://brew.sh/)

```
brew install git
```

#### Instalador

http://git-scm.com/download/mac

### Windows

* http://git-scm.com/download/win
* https://git-for-windows.github.io/
* https://desktop.github.com/

## Primer proyecto con git

Si aún no has podido instalar, ve a:

http://try.github.io

```
$ mkdir project && cd project
$ git init
Initialized empty Git repository in .../project/.git
```

```
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

### ¿`branch master`? ¿`commit`?

Paciencia...

### Añadiendo un archivo

```
$ echo "Hola git" > hola.txt
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    hola.txt
```


