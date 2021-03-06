# campusciff
Ejercicios propuestos en https://github.com/asanzdiego/curso-git-github-markdown-2015/blob/master/slides/md/git-github-markdown-ejercicios.md

A continuación, se resuelven punto por punto cada uno de los apartados indicados.

## Repositorio campusciff (I)

### Crear un repositorio en vuestro GitHub llamado campusciff.
Para crear un repositorio, basta con iniciar sesión en nuestra cuenta de GitHub y pulsar sobre el botón 'Create repository' que se encuentra en el desplegable mostrado al pulsar sobre '+', en la esquina superior derecha. A continuación, introduciremos una descripción del proyecto (opcional).

En la imagen se aprecia con detalle la operación:

![crear_repositorio](images/crear_repo.png)

GitHub ofrece la posibilidad de crear directamente el fichero README desde la propia interfaz, por lo que aprovecharemos esta ayuda.
Es posible también añadir un fichero .gitignore, que servirá más tarde para que Git mantenga fuera del control de versiones los ficheros que le indiquemos.
Los repositorios pueden ser públicos o privados, siendo esta última opción de pago.

## Repositorio campusciff (II)

### Clonar vuestro repositio en local.
Para clonar el repositorio en nuestro ordenador y comenzar a trabajar utilizaremos el comando 'clone':
```bash
> git clone https://github.com/elelement/campusciff.git
```

### Commit inicial
En la siguiente imagen se aprecia mejor el flujo de trabajo que sigue Git:
![flujo_1](http://rogerdudler.github.io/git-guide/img/trees.png)

Para subir los cambios de nuestra zona de trabajo al repositorio local, primero tendremos que añadirlos a la zona de 'staging', es decir, añadirlos al control de versiones (comando 'add'; aplicable a todos los ficheros que sean nuevos o que presenten cambios). A continuación, utilizaremos el comando 'commit' para confirmar dichos cambios en el repositorio (la sincronización con repositorios remotos es otra cuestión):
```bash
> git status
```

Nos muestra como efectivamente el fichero ha sido modificado por los cambios presentes hasta este punto respecto de la creación del fichero README.
```
C:\ejercicios_github\campusciff>git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        images/

no changes added to commit (use "git add" and/or "git commit -a")
```

Añadimos el fichero a la zona intermedia:
```bash
> git add .
> git status
```

Los ficheros ya se encuentan listos para ser publicados en el repositorio local:
```
C:\ejercicios_github\campusciff>git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md
        new file:   images/crear_repo.png
        new file:   images/git_status_unstaged.png
```

Finalmlente, realizamos el commit. La opción '-m', permite introducir un mensaje que describa el propósito del commit:
```bash
> git commit -m "commit inicial"
```

### Push inicial
Para sincronizar nuestros cambios con el repositorio remoto, tendremos que utilizar el comando 'push' sobre la rama que deseemos. En este caso sólo hay una, 'master', que se crea por defecto al crear el repositorio en Github.
```bash
> git push origin master
```

### Ignorar archivos (I)

1. Crear en el repositorio local un fichero llamado privado.txt:
```bash
touch privado.txt
```

2. Crear en el repositorio local una carpeta llamada privada.
```bash
mkdir privada
```

### Ignorar archivos (II)

Realizar los cambios oportunos para que tanto el archivo como la carpeta sean ignorados por git. Para ello, crearemos un fichero llamado '.gitignore' añadiendo las entradas a ignorar:
```bash
privada/
privado.txt
git add .gitignore
git commit -m "Añadido fichero .gitignore" .gitignore
```

### Añadir fichero 1.txt
```bash
type NUL > 1.txt
git add 1.txt
git commit -m- "Fichero 1.txt creado" 1.txt
```

### Crear etiquetas
Para crear una etiqueta o 'tag', utilizaremos el comando 'tag':

#### Tag v0.1
```bash
git tag -a v0.1 -m "Nuevo tag v0.1"
```

#### Subir el tag v0.1
```bash
git push --tag origin master
```
La consola nos debería mostrar el éxito de la operación.

### Crear una rama v0.2
Para crear una rama utilizaremos el comando 'branch':
```bash
git branch v0.2
```
Nos posicionaremos dentro de esta nueva rama utilizando el comando 'checkout' 
> Los cambios no guardados en la rama actual se perderán!
```bash
git checkout v0.2
```
Añadimos el fichero 2.txt
```bash
type NUL > 2.txt
git add .
git commit -m- "Fichero 2.txt creado" .
```

Y subimos los cambios al repositorio remoto:
```bash
git push origin v0.2
```

### Merge
Para fusionar cambios utilizaremos el comando 'merge'. Existen varios tipos de 'merge' o fusiones.

#### Merge directo
1. Posicionarse en la rama master
```bash
git checkout master
```
2. Hacer un merge de la rama v0.2 en la rama master
```bash
git merge v0.2 -m "Merge de la rama v0.2 en la rama master"
```

#### Merge con conflicto (I)
En la rama master poner "Hola" en el fichero 1.txt y hacer commit:
```bash
echo "Hola" > 1.txt
git add .
git commit -m "Conflicto parte I" .
```

#### Merge con conflicto (II)
Posicionarse en la rama v0.2 y poner "Adiós" en el fichero "1.txt" y hacer commit:
```bash
git checkout v0.2
echo "Adios" > 1.txt 
git add .
git commit -m "Conflicto parte II" .
```

#### Merge con conflicto (III)
Posicionarse de nuevo en la rama master y hacer un merge con la rama v0.2:
```bash
git checkout master
git merge v0.2 -m "Conflicto v0.2 en master resuelto"
```

### Listado de ramas
1. Listar las ramas con merge:
```bash
git branch --merged
```
2. Listar las ramas sin merge:
```bash
git branch --no-merged
```

### Resolver el conflicto 
Si mostramos el contenido del fichero 1.txt podemos observar que existe un conflicto:
```
<<<<<<< HEAD
Hola
=======
Adios
>>>>>>> v0.2
```
Lo que queda entre el "<<<<<<< HEAD" y el "========" es lo que había en la rama master y lo que encierra "========" y ">>>>>>> v0.2" lo que tiene la rama v0.2.

Editamos el fichero a mano y dejamos el mensaje "Adios" únicamente:
```
Adios
```

Subimos el fichero al repositorio local utilizando el comando:
```bash
git commit -i 1.txt
``` 
o, alternativamente:
```bash
git commit -am 1.txt
``` 

### Borrar ramas
Antes de borrar la rama v0.2, crearemos un tag con el mismo nombre:
1. Crear un tag:
```bash
git tag -a v0.2 -m "Creado tag v0.2"
```
2. Y borramos la rama
```bash
git branch -d v0.2
```
Para borrar la rama remotamente podemos utilizar el siguiente comando:
```bash
git push origin --delete v0.2
```

### Listado de cambios
Listar los distintos commits con sus ramas y sus tags:
```bash
> git log --oneline --decorate --graph
```
La consola nos muestra lo siguiente:
```
*   047c3f6 (HEAD -> master, tag: v0.2) Conflicto arreglado
|\
| * e32b95b Conflicto parte II
* | ee722c7 Añadidas modificaciones README
* | dc634e8 Conflicto parte I
|/
* 82d2dcd (origin/v0.2) Nuevos cambios en README hechos antes del merge que necesitan ser subidos
* a644807 Fichero 2.txt creado
* e552239 (tag: v0.1, origin/master, origin/HEAD) Modificaciones README
* c71492a Fichero 1.txt creado
* 3122d3e Añadido fichero .gitignore
* be602dd commit inicial
* fd0f55f Initial commit
```

Se puede crear un alias para esta operación:
```bash
git config --global alias.list 'log --oneline --decorate --graph -all'
```

## Cuenta de GitHub
Aspectos relacionados con nuestra cuenta de GitHub

### Poner una foto en vuestro perfil de GitHub.
Basta con ir a la pestaña de "Profile", en la sección de configuración o "Settings".
![imagen_perfil](images/imagen_perfil.png)

### Poner el doble factor de autentificación en vuestra cuenta de GitHub.
Basta con activarla desde la pestaña "Security":

![imagen_perfil](images/dos_pasos.png)

### Añadir (si no lo habéis hecho ya) la clave pública que se corresponde a tu ordenador.
Ya fue añadida con anterioridad:
![imagen_perfil](images/clave.png)

## Uso social de GitHub
En esta sección se detalla el aspecto social de GitHub.

### Preguntar los nombres de usuario de GitHub de tus compañeros de clase, búscalos, y sigueles.
![imagen_perfil](images/following.png)

### Seguir los repositorios campusciff del resto de tus compañeros.
![imagen_perfil](images/watching.png)

### Añadir una estrella a los repositorios campusciff del resto de tus compañeros.
![imagen_perfil](images/starred.png)

## Tablas
Crear una tabla de este estilo en el fichero README.md con la información de varios de tus compañeros de clase:

|NOMBRE|GITHUB|
|--------|--------|
|Macarena Garañena|https://github.com/macarenagaranena|
|Daniel Escuder|https://github.com/Danielobit|
|Alejandro Díaz|https://github.com/adiazgalache|
|Aldolfo Sanz|https://github.com/asanzdiego|

## Colaboradores
Poner a github.com/asanzdiego como colaborador del repositorio campusciff:

![collaborators](images/collaborators.png)

## Crear una organización
Crear una organización llamada campusciff-tunombredeusuariodegithub:

![organization](images/organization.png)

## Crear equipos
1. Crear 2 equipos en la organización campusciff-tunombredeusuariodegithub, uno llamado administradores con más permisos y otro colaboradores con menos permisos.
**Administradores**
![team_admins](images/team_admins.png)
**Colaboradores**
![team_colaboradores](images/team_colaboradores.png)

2. (y 3.) Meter a github.com/asanzdiego y a 2 de vuestros compañeros de clase en el equipo administradores y colaboradores.
![team_miembros](images/team_miembros.png)

## Crear un index.html
Se crea un index.html dentro del repositorio y se accede a la web introduciendo campusciff-elelement.github.io:
![web_org](images/web_org.png)

## Crear Pull Requests
1. Hacer 2 forks de 2 repositorios campusciff-tunombredeusuariodegithub.github.io de 2 organizaciones de las que no seais ni administradiores ni colaboradores:
![forks](images/forks.png)

2. Creamos una rama en cada fork
Rama b-jmcabrera creada para cada repositorio

3. Pull requests
![pull_requests](images/pull_requests.png)

## Gestionar Pull Requests
![pull_requests_2](images/pull_requests_2.png)

