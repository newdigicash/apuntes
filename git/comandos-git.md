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
0.8

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
`git fetch` | descarga cambios remoto en rama oculta **origin/master**
`git merge origin/master` | fusiona cambios de **origin/master** con rama actual
`git pull` | descarga el último commit. alias de **git fetch \+ git merge**
`git pull origin _rama_` | descarga el último commit de la _rama_
`git remote -v` | muestra todos los repos remotos
`git status` | muestra cambios repo local
`git add *` | agrega cambios index repo local
`git mv -f _origen_ _destino_` | mueve archivos y carpetas de \_origen_ a \_destino_
`git rm -rf dir-local` | borra archivos del index remoto
`git commit -m "mensaje"` | guarda cambios repo local
`git push origin master` | guarda cambios en rama repo remoto 
`git branch _rama_` | crea una rama
`git branch -a` | muestra todas las ramas locales
`git log --all --graph --decorate --oneline` | listado de commits de todas ramas
`git checkout _rama_` | cambia de rama
`git checkout -b _rama_` | crea y cambia a la rama nueva
`git checkout -b _rama_ _existente_` | crea [rama nueva a partir de rama existente][gitOtraBranch]
`git merge _rama_` | fusiona \_rama_ a la rama actual con todos los commits
`git merge --squash _rama_` | fusiona la \_rama_ a la actual para un solo commit
`git remote update origin --prune` | actualiza lista de ramas remotas y borra inexistentes
`git branch -D _rama_` | borra una rama
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
[gitOtraBranch]: https://stackoverflow.com/questions/4470523/create-a-branch-in-git-from-another-branch
