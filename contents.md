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

#### Commit

Para que git se haga cargo de tus archivos tienes que añadirlos explícitamente
(`git add`).

Para que se guarden los cambios realizados sobre un archivo hay que
confirmar dichos cambios (`git commit`).

#### Branch

Git guarda los commits en un **grafo acíclico unidireccional** (vamos, que *cada
commit tiene un padre*). Eso permite que varios commits tengan un mismo padre,
es decir, que se diverja. Cada divergencia de la rama por defecto (`master`) es
un branch (*más o menos, detached head, etc...*), y el último commit realizado a
un branch se llama `HEAD` (salvo cuando te encuentras en *detached head*).

Los branchs tienen nombre, y se crean con `git branch <nombre>`.

Para cambiar de un branch a otro se usa `git checkout <nombre>`.

El comando `git checkout -b <nombre>` es un alias para `git branch <nombre> &&
git checkout <nombre>`

### Añadiendo un archivo

Si creamos un archivo, git se da cuenta de que todavía no tiene control sobre
ese archivo, y nos lo muestra al usar `git status`.

```
$ echo "Hola git" > hola.txt
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    hola.txt
```

Cuando usamos `git add <nombre-del-archivo>`, git guarda los cambios en lo que
llama *staging area*. Todavía los cambios no están confirmados.

```
$ git add hello.txt
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   hello.txt
```

Al usar `git commit`, confirmamos los cambios añadidos. Cada commit va
acompañado de un mensaje, que se puede especificar con el flag `-m`, o escribir
en el editor de nuestra preferencia.

```
$ git commit -m "Add hello.txt"
[master (root-commit) deb85ac] Add hello.txt
 1 file changed, 1 insertion(+)
  create mode 100644 hello.txt
```

¡Genial! Ya hemos creado nuestro primer commit.

### Viendo la historia de nuestro repositorio

Para ver la historia de nuestro repositorio podemos usar `git log`. Ahora sólo
tenemos un commit así que va a ser una historia un poco cutre:

```
commit deb85ac1a081a5ccacfa9a813f497d61577522e1
Author: Emilio Cobos Álvarez <ecoal95@gmail.com>
Date:   Sat Oct 24 17:01:14 2015 +0200

    Add hello.txt
```

### Realizando más cambios

Nuestro fichero `hello.txt` es un poco cutre, vamos a cambiarlo.

Además, queremos generar a partir de él un pdf, así que a ver cómo podemos
hacerlo:

```
$ echo "Git rules" >> hello.txt
$ cat > Makefile
hello.pdf: hello.txt
  wkhtmltopdf $< $@
$ make
wkhtmltopdf hello.txt hello.pdf
Loading pages (1/6)
Counting pages (2/6)
Resolving links (4/6)
Loading headers and footers (5/6)
Printing pages (6/6)
Done
```

Hey, **parece que ha funcionado**, vamos a añadir los cambios!

```
$ git add --all
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  new file:   Makefile
  new file:   hello.pdf
  modified:   hello.txt
```

Jum... Hemos estado a punto de hacer commit de nuestro archivo pdf, que no
queremos que esté en nuestro repositorio.

Para eliminarlo de la *staging area* podemos usar:

```
$ git reset HEAD hello.pdf
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  new file:   Makefile
  modified:   hello.txt

  Untracked files:
  (use "git add <file>..." to include in what will be committed)

    hello.pdf
```

Eso está mejor, confirmemos los cambios:

```
$ git commit -m "Modify hello.txt and allow converting to pdf"
[master 2a1c2dd] Modify hello.txt and allow converting to pdf
 2 files changed, 3 insertions(+)
  create mode 100644 Makefile
```

### ¿Cómo podemos evitar añadir un archivo por error?

Git antes de añadir cambios lee un archivo llamado `.gitignore`. Ignora todos
los archivos o directorios que se especifiquen en él. Es perfecto para
**archivos compilados o autogenerados, logs, credenciales...**

Vamos a usarlo para evitar que nuestro archivo `hello.pdf` sea añadido al
repositorio:

```
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    hello.pdf

    nothing added to commit but untracked files present (use "git add" to track)
$ cat > .gitignore
hello.pdf
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    .gitignore

    nothing added to commit but untracked files present (use "git add" to track)
```

Genial! Parece que git ya ignora nuestro archivo `hello.pdf`. Vamos a guardar
los cambios!

```
$ git add --all # Ahora podemos usar --all estando seguros de que hello.pdf no será añadido
$ git commit -m "Add .gitignore"
[master e32ad43] Add .gitignore
1 file changed, 1 insertion(+)
  create mode 100644 .gitignore
```

### Subiendo los cambios a un repositorio remoto

Parece que nuestro **gran proyecto** (/joke) ha alcanzado un estado en el que
merece la pena compartir los cambios, o simplemente guardarlos en un sitio
remoto para no perderlo.

Vamos a usar github como ejemplo, aunque se podrían usar cualquier otra
plataforma (gitlab, bitbucket...) o incluso un servidor propio.

Una vez creemos nuestro repositorio, podemos hacer:

```
$ git remote add origin git@github.com:user/repo.git
$ git push origin master
```

El primer comando añade un *remote* a nuestro repositorio, llamado `origin`, y
con la url git@github.com:user/repo.git.

El segundo hace `push` (*empuja* los cambios) del branch actual al branch
`master` del repositorio remoto llamado `origin`.

### Viendo los cambios realizados a través de la historia

GitHub ofrece una interfaz muy amigable para ver los cambios entre commits...
Pero no es nada que no podamos hacer desde la shell!

```
$ git diff # Muestra los cambios hechos sobre la staging area
$ git diff HEAD # Muestra los cambios sin confirmar
$ git diff HEAD^ HEAD # Muestra los cambios del último commit
$ git diff <hash-1> <hash-2> # Muestra los cambios entre el commit <hash-1> y el commit <hash-2>
$ git diff branch1..branch2 # Muestra las diferencias entre ambos branchs
```

Algunas notas acerca de los nombres de commits:

* `HEAD`, como ya habíamos dicho, representa el último commit hecho al branch
    actual, o el commit al que hagamos hecho checkout.
* El símbolo `~` se puede usar para especificar commits anteriores en la
    historia. Así `HEAD~2` será el antepenúltimo commit, `HEAD~3` el anterior a
    ese, y así sucesivamente.
* El símbolo `^`, representa el commit anterior, por lo tanto `HEAD^` es sólo un
    alias para `HEAD~1`, que será el penúltimo commit.
