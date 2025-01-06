# Git

>La consola de git usa como base el sistema de **Unix**, por ende los comandos para navegar entre directorios son iguales.

---
### Configuración inicial

Para comenzar a trabajar con git es necesario establecer la siguiente configuración
``` git
git config --global user.name ¨iV4sh¨
git config --global user.email ¨michaelenriquez525@gmail.com¨
```
Esto con el propósito de establecer un usuario de manera global en el equipo.

### Conceptos de control de versiones

Aplicar el comando `git init` inicializara una carpeta llamada **.git**, en ella se registraran todos los cambios hechos para los directorios o ficheros seleccionados.
+ El comando también creara un `defaultBranch` (rama por defecto) bajo el nombre de <font color="#4bacc6">Main</font> o <font color="#4bacc6">Master</font>.
+ Con esto ya tendremos nuestro repositorio listo.

**`git status`** : Este comando nos muestra el estado de la rama en la que nos encontramos, así como los archivos no añadidos al repositorio, los ficheros modificados y los que están listos para aplicar un <font color="#4bacc6">commit</font>.

#### Pasos para un Commit

Hay que entender los conceptos de add y commit.
**`git add <fichero>`**: Este comando añade el estado actual de un fichero al repositorio como si de una versión se tratara.
**`git commit -m ¨message¨`**: Guarda el estado actual del repositorio con un mensaje descriptivo.
Ejemplo ⬇️
```git
# Añadimos los ficheros (git add . para añadir todos)
$ git add .

# hacemos el commit del estado actual
$ git commit -m "actualización de estado"
[main (root-commit) 002d571] actualización de estado
 1 file changed, 1 insertion(+)
 create mode 100644 HelloGit.py
```
Observamos como hacer un commit nos indica los cambios realizados y un **Hash** que seria el identificador de ese commit.
#### Logs

El cambio se puede observar al ejecutar el comando `git log`.
``` git
$ git log
commit 002d571b73b3d9aa68b991eed9e22ff1e9b93d8e (HEAD -> main)
Author: ¨iV4sh¨ <¨michaelenriquez525@gmail.com¨>
Date:   Sun Jan 5 11:24:20 2025 -0500

    actualización de estado
```
Este comando nos da la información de cuando y quien hizo el commit además del Hash y el mensaje asignados este. Actúa como una especie de historial de cambios.
Existe también el comando `git reflog` que nos muestra un historial incluyendo cambios entre ramas, resets, etc.
``` git
$ git reflog
	518d1d5 (HEAD) HEAD@{0}: reset: moving to 518d1d59d5854a5de843e091470db2453b3f2720
	2e73272 (main) HEAD@{1}: checkout: moving from 002d571b73b3d9aa68b991eed9e22ff1e9b93d8e to 2e732722d1e8862607a4a410f92757390539ce91
	002d571 HEAD@{2}: checkout: moving from main to 002d571b73b3d9aa68b991eed9e22ff1e9b93d8e
	2e73272 (main) HEAD@{3}: commit: Se añada el .gitignore
	518d1d5 (HEAD) HEAD@{4}: commit: Commit de un archivo modificado
	0bd9426 HEAD@{5}: reset: moving to HEAD
	0bd9426 HEAD@{6}: commit: Segundo Commit
	002d571 HEAD@{7}: commit (initial): actualización de estado
```

#### Verificación de versiones

**`git checkout <fichero>`**: Comando que puede servir para revisar algún commit especifico sin afectar el estado actual del repositorio o para restaurar los ficheros a un commit pasado:
``` git
# Restaurar fichero
$ git checkout HelloGit.py
	Updated 1 path from the index

# Revison de un commit anterior (A este podemos mandarlo a otra rama o resetar el repositorio en este punto)
$ git checkout 002d571b73b3d9aa68b991eed9e22ff1e9b93d8e
```
**`git reset`**: Se utiliza para deshacer cambios en el historial de commits, tiene 3 modos soft, mixed y hard.
```git
# Muestra para resetear a un commit anterior o para volver a un commit futuro
$ git reset --hard  518d1d59d5854a5de843e091470db2453b3f2720
	HEAD is now at 518d1d5 Commit de un archivo modificado
```
**`git diff`**: Al modificar un fichero este comando nos aporta un visión de las diferencias entre una versión anterior a la recientemente modificada:
``` git
$ git diff
	diff --git a/HelloGit.py b/HelloGit.py
	index 3dadacf..cf9d379 100644
	--- a/HelloGit.py
	+++ b/HelloGit.py
	@@ -1 +1 @@
	-print("Cambio de texto :v")
	\ No newline at end of file
	+print("Ahora el texto es diferente")
	\ No newline at end of file
```

### Trabajar con Ramas

>Un repositorio puede componerse de tantas ramas como sea requerido, estas son bifurcaciones en las que se puede ir trabajando independientemente de la rama principal. Usadas comúnmente para independizar el trabajo de cada una de las personas que trabajan en el repositorio
#### Crear una rama

Para crear una nueva rama podemos aplicar el comando `git branch <Nombre de la rama>`
Una vez ejecutado el comando se creara una copia del commit actual y los logs, pero, en un flujo de trabajo nuevo bajo el nombre de la nueva rama.
Para trabajar en otra rama debemos hacer el cambio mediante el comando `git switch <Nombre de la rama>`. De esta manera el HEAD apuntara a la nueva rama.
``` git
$ git branch Rama1

$ git switch Rama 1
	Switched to branch 'Rama1'

$ git log
	commit 2e732722d1e8862607a4a410f92757390539ce91 (HEAD -> Rama1, main)
```
#### Unir Ramas

Podemos usar el comando `git merge` para combinar los cambios de dos ramas:
```  git
git merge main

	Merge made by the 'ort' strategy.
	 HelloGit2.py | 2 +-
	 1 file changed, 1 insertion(+), 1 deletion(-)
```
El unión entre ramas puede visualizarse mediante el log de la siguiente manera:
``` git
$ git log --graph --decorate --all --oneline
	*   384cd6d (HEAD -> Rama1) Merge branch 'main' into Rama1
	|\
	| * e527cbc (main) Actualizacion de version
	* | 80370c4 Primer commit en la rama1
	|/
	* 2e73272 Se añada el .gitignore
```
Siempre y cuando el equipo encargado de la Rama no haga cambios en los ficheros asignados a otra Rama no habrá **conflictos** entre las versiones, de existir conflictos hacer un `git merge` puede incurrir en muchos errores.
Para *solucionarlo* se deben deshacer los cambios incorrectos en los ficheros que se haya generado el conflicto.
#### Eliminar Ramas

Para eliminar ramas basta con ejecutar el comandos `git branch -d Rama1`
##### Git stash

Este es un comando especial que nos permite guardar el proceso o un trabajo inconcluso en una rama, para cambiar a otra sin la necesidad de hacer un commit de los cambios.
``` git 
$ git stash
	Saved working directory and index state WIP on Rama1: 384cd6d Merge branch 'main' into Rama1

# Para chequear los trabajos pendientes
$ git stash list

# Para recuperar los trabajos pendientes
$ git stash pop
```
### Git Ignore

Git ignore es un archivo que registra las rutas, directorios y/o ficheros que no queremos tener en cuenta en nuestro repositorio.
Para esto necesitamos crear el archivo bajo el nombre especifico de `.gitignore`, para agregar ficheros se debe seguir la siguiente nomencaltura:
``` git
**/.<Ruta o Nombre del fichero>
```
### Atajos

En git podemos crear alias para ejecutar comandos específicos mediante el uso de otra instrucción
``` git
$ git config --global alias.<Nombre del nuevo comando> "<Comando>"

$ git config --global alias.tree "log --graph --decorate --all --oneline"
$ git tree
	* 518d1d5 (HEAD -> main) Commit de un archivo modificado
	* 0bd9426 Segundo Commit
	* 002d571 actualización de estado
```

---

# GitHub

> Usa como base Git Pero de forma remota, pues esta se encuentra situada en en la nube. Así por una parte podemos versionar nuestro trabajo como hacerlo publico. Además de que nos provee de muchas mas herramientas para la gestión de proyectos.

---
### Git local a Github

Necesitaremos tener un repositorio creado en github para mediante el comando `git remote...` crear un enlace entre tu entorno de trabajo local y el entorno remoto de github.
``` git
git remote add origin <URL del repositorio>
```
Una vez aplicado ese comando podemos subir nuestro repositorio a github de la siguiente manera:
``` git
$ git push -u origin main
```
`origin` representa el repositorio local y `main` el repositorio remoto.
#### Git fetch y pull

> Al trabajar de esta manera pueden ocurrir incongruencias ya que se puede hacer commits en la parte local como remota. 

Usamos el comando `git fetch` para descargar el historial de cambio entre las dos partes, pero no descarga los cambios.
Por otra parte `git pull` descarga el historial de cabios y también los cambios, es decir de tener un commit en github este se actualizara en git. en caso de que falle, nos pedirá solucionar un conflicto haciendo merge entre la rama local y la remota (el comando ya te lo sugiere).
``` git
# Para sincronizar el historial de cambios
$ git fetch

# Para sincronizar los cambios
$ git pull
	Merge made by the 'ort' strategy.
	 README.md | 1 +
	 1 file changed, 1 insertion(+)
	 create mode 100644 README.md
```
#### Fork

**Fork** sirve para hacer una copia de un repositorio ajeno de github, y trabajarlo a partir desde el punto de su ultimo commit. Esto no modifica el repositorio original pero el creador puede tomar en cuanta los cambio que tu hagas y hacerlos parte de su repositorio mediante la opción de ``Sync Fork`` o ``Pull request``.
Esta configuración solo se puede hacer desde github.
