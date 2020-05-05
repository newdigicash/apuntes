# Comandos de Git
Resumen de comandos más usados de git

Para crear un repositorio y clonarlo, revisar [este enlace][gitInicio]. 
Hay una [guia genial][gitTutorial] de comandos [aquí][gitTutorial].  

[Tutorial interactivo][gitInteractivo] sobre ramas [aquí][gitInteractivo]. 
La explicación de las [diferencias entre fetch+merge y pull][gitPull] está [aquí][gitPull].
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.4

## 2. Observación
En proyectos pequeños está bien usar el comando **git pull**. 
Mientras que en proyectos grandes es mejor usar **git fetch**, 
revisar los cambios, y luego unir con **git merge**.

## 3. Contenido 
Listado de comandos

Cmd | Descripción
:-- | :--
`git config --global user.name "nombre"` | asigna el nombre de usuario global
`git config --global user.email correo` | asigna email global
`git clone _url-publica_` | descarga repo en carpeta actual
`git clone _url-publica_ _dir-local_` | descarga repo en carpeta nueva
`git fetch`  | descarga cambios remoto en rama oculta **origin/master**
`git merge origin/master` | fusiona cambios de **origin/master** con rama actual
`git pull` | descarga el último commit. alias de **git fetch \+ git merge**
`git remote -v` | muestra todos los repos remotos
`git status` | muestra cambios repo local
`git add *` | agrega cambios index repo local
`git commit -m "mensaje"` | guarda cambios repo local
`git push origin master` | guarda cambios en rama repo remoto 
`git branch _rama_` | crea una rama
`git branch -a` | muestra todas la ramas locales
`git checkout _rama_` | cambia de rama
`git checkout -b _rama_` | crea y cambia a la rama nueva
`git branch -d _rama_` | borra una rama
`git merge _rama_` | fusiona \_rama_ a la rama actual
`git rebase _rama_` | commit a _rama_ copiando de rama actual
`git checkout HEAD^` | ref. relativa, revisa el commit anterior
`git branch -f _rama_ HEAD~numero` | mueve la rama n veces atrás
`git reset HEAD~1` | regresa al commit anterior **solo repo local**
`git revert HEAD` | commit nuevo sin aplicar último commit en **remoto**
`git cherry-pick c1 c2... ` | copia los n commits a la rama actual
`git rebase -i HEAD~numero` | selecciona commits y copia desde head actual
`git tag _tag_ _commit_` | asigna etiqueta al commit donde esta HEAD
`git describe _rama_` | muestra info de tag_commits_hash de rama o HEAD

## 4. Fuentes
Libro de git: <https://git-scm.com/book/en/v2>

Configuración inicial: <https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup>

[//]: # (referencias citadas)
[gitInicio]: https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository
[gitTutorial]: https://rogerdudler.github.io/git-guide/index.es.html
[gitInteractivo]: https://learngitbranching.js.org/?locale=es_ES
[gitPull]: https://blog.artegrafico.net/git-fetch-y-git-pull-diferencias-y-formas-de-uso
