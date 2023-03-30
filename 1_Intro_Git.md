# Nota

# Índice

- [Nota](#nota)
- [Índice](#índice)
- [Introducción](#introducción)
- [¿Qué es el control de versiones?](#qué-es-el-control-de-versiones)
  - [Acciones posibles con git](#acciones-posibles-con-git)
  - [Ventajas de git](#ventajas-de-git)
  - [Terminología de Git](#terminología-de-git)
- [Ejercicio: Prueba de Git](#ejercicio-prueba-de-git)
  - [Creando el primer repositorio](#creando-el-primer-repositorio)
  - [Empezar un nuevo repositorio local](#empezar-un-nuevo-repositorio-local)
- [Comandos básicos de Git](#comandos-básicos-de-git)
- [Prueba de conocimientos](#prueba-de-conocimientos)
- [Resumen](#resumen)



# Introducción

Si no conoces git entonces imagina lo siguiente:
Una herramienta con la cual puedas tener puntos de guardado sin necesidad e hacer *control+z* y poder consultar cada cambio hecho en un archivo y registrar el autor y los cambios en epecifico y mejor aún una descripción con la que especifiquen qué hizo esa persona e incluso aún mejor trabajar en paralelo en ese archivo es decir un amigo tiene la misma versión que tú y tú tienes el mismo pero puedes ir y ver los cambios de tu amigo y el también los tuyos, pues todo eso y más con servicios en la nube puede hacer git.


# ¿Qué es el control de versiones?

- VSC (Sistema de control de versiones)
- Este es un programa que genera un seguimiento de los cambios de un conjunto de archivos o una carpeta que contenga archivos.
- Permite la recuperación sencilla de versiones anteriores.
- **SCM** (Sistema de administración de configuración de software) es otro nombre para un VSC.
- [Documentación de git](https://git-scm.com/)


## Acciones posibles con git
- Registro de cambios realizados a un proyecto, cuándo se hicieron, autor, y mensaje que contiene la razón del cambio.
- Recuperación de versiones y comparación de versiones.
- Ramas de trabajo que son identicas y en la cual cada desarrollador puede trabajar de manera independiente pero depentiende de una rama principal llamada ***main***.
  
  **NOTA**: El autor de git fue Linux Torvalds, creador de Linux.

  ![Papá de Linux](https://imagenes.elpais.com/resizer/L6qeJF-8AI_3y6oboKR2KxBI76o=/1200x0/cloudfront-eu-central-1.images.arcpublishing.com/prisa/G7XBAG3ZGD7IWLG4OT2GA4XBEM.jpg)

## Ventajas de git

- Sistemas de control anteriores usaban servidor centralizado para almacenar el historial de un proyecto. **Git es un sistema *distribuido* es decir cliente y servidor almacenan el hisotial**
- Al ser **distribuido** es posible el trabajo sin conexión y subir cambios una vez reconectado a la red.
- **NOTA.** No es necesario un servidor porque el historial ya se almacena localmente y por lo tanto se puede pasar el proyecto por cualquier medio posible y se tendría el historial **pero git fue creado justamente para evitar lo anterior.**

## Terminología de Git

Hay determinadas palabras que se usan frecuentemente en git para señalar errores y escribir comando para guardar cambios por lo que es indispensable conocerlos.

- **Árbol de trabajo**
  - Conjunto de archivos o carpetas dentro de una carpeta donde se trabaja con git.
- **Repositorio**
  - Coloquialmente llamado ***repo*** es donde esta el árbol de trabajo y normalmente se identifica ya que al ver las carpeetas ocultas hay un carpeta ***.git*** 
- **Hash**
  - Número generado por una función hash que representa el contenido del archivo. **Git usa hashes de 160 bits de longitud**. La ventaja de utilizar esa longitud es la posibilidad de implementar un algoritmo y detectar los cambios.
    - Nota: Si se cambia la marca de fecha y hora del archivo, pero el hahs del archivo no, git sabe que el contenido del archivo no se ha modificado.
- **Objeto**
  - Un respositorio de **Git contiene cuatro objetos**, cada uno identificado de forma única por un hsh SHA-1. 
    - Un objeto *blob* contiene un archivo normal.
    - *árbol* representa un directorio.
    - *confirmación* versión en especifica del árbol de trabajo
    - *etiqueta* nombre asosiado a una confirmación
- **Confirmación**
  - Cuando se usa **confirmar** significa que lo toma y va a la base de datos para que lo cambios realizados puedan ser vistos por otros usuarios.
- **Rama**
  - Serie ocn nombre de confirmaciones vinculadas.
  - La rama predeterminada, que se crea al inicializar un repositorio, se denomina *nivel superior*. Este nivel se le denomina *HEAD* 
  - Esto permite que cada desarrollador cuente con una rama de trabajo y contribuyan de manera independiente o conjunta cuando se haga una fusión de los cambios pero esta siempre depende de la rama principal por lo que deben estar al tanto de las actualizaciones que reciba la rama principal.
- **Remoto**
  - "referencia con nombre a otro respositorio Git. Cuando se crear un repositorio, Git crear un remoto denominado *origin*, que es el remoto predeterminado par ala operaciones de envío e incorporación de cambios."
- **Comandos, subcomandos y opciones**
  - Las operaciones de git se realizan mediante comandos como 
    - > git push
    - > git pull
    - **git es el comando** 
    - *El subcomando especifica la operación que quiere que Git relice.*
    - Los **subcomandos** suelen ir acompañados de opciones, que usan guines (-) (--) por ejemplo *git commit -m ""*

**NOTA:** *en los siguientes temas entenderás los comandos de git o si no consulta el glosario de*

# Ejercicio: Prueba de Git

## Creando el primer repositorio

1. Instala git en tu computadora [downloads](https://git-scm.
2. com/downloads)
3. Ejecuta el siguiente comando de git 
   1. Linux y macOS
      1. Abre la terminal y ejecuta
      2. > git --version
   2. Windows
      1. Abre git bash y ejecuta 
      2. > git --version
4. Debería ver na salida similar a este ejemplo:
   > git version #.#.#
5. Para configurar Git, debe definir algunas variables globales de nombre y correo. Ambas necesarias para realizar cambios y confirmaciones.
   1. git config --global user.name "<USER_NAME>"
   2. git config --global user.email "<USER_EMAIL>"
   3. **Para observar los cambios realizados ejecute el siguiente comando:**
      1. > git config --list

## Empezar un nuevo repositorio local

1. Crear una nueva carpeta a través de la terminal con el comando:
   1. mkdir *"nombre_carpeta"*
2. Desplazarse hacia la carpeta creada con el comando
   1. cd *"nombre_carpeta"*
3. Ahora se inicializará el reposotio y estableerá el nombre d ela rama predeterminada main:
   1. Asumiento esta por encibma de la versión 2.28.0 de Git ejecute el siguiente comando:
      1. git init --initial-branch=main
      2. git init -b main
4. Para saber el estado del arbol ejecute 
   1. git status
   2. La salida que tendrá será una pareceda a la siguiente
```
On branch master <br>

No commit yet

nothng to commit (create/copy fules and use "git add" to track)

```
5. Use e comando ls para mostrar el estado del árbol de trabajo:
   1. ls -a 


# Comandos básicos de Git

# Prueba de conocimientos 

# Resumen
