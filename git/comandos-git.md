# Comandos de Git
Resumen de comandos más usados de git
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.2

## 2. Observación

Para crear un repositorio y clonarlo, revisar [este enlace][gitInicio]. 
Hay una [guia][gitTutorial] genial de comandos [aquí][gitTutorial]

[Tutorial interactivo][gitInteractivo] sobre ramas [aquí][gitInteractivo]
## 3. Contenido 
Listado de comandos

Cmd | Descripción
:-- | :--
`git clone _url-publica_` | descarga repo en carpeta actual
`git clone _url-publica_ _dir_local_` | descarga repo en carpeta nueva
`git pull` | descarga el ultimo commit de rama actual
`git status` | muestra cambios repo local
`git add *` | agrega cambios index repo local
`git commit -m "mensaje"` | guarda cambios repo local
`git push origin master` | guarda cambios repo remoto 
`git branch _rama_` | crea una rama
`git checkout _rama_` | cambia de rama
`git checkout -b _rama_` | crea y cambia a la rama nueva
`git branch -d _rama_` | borra una rama
`git merge _rama_` | fusiona rama actual a \_rama_
`git rebase _rama_` | base de rama actual a \_rama_ 
`git checkout HEAD^` | ref. relativa, revisa el commit anterior
`git branch -f _rama_ HEAD~numero` | mueve la ram n veces atras
`git reset HEAD~1` | regresa al commit anterior **solo repo local**
`git revert HEAD` | commit nuevo sin aplicar ultimo anterior  en **remoto**
`git cherry-pick c1 c2... ` | copia los n commits a la rama actual
`git rebase -i HEAD~numero` | seleccion commits y copia desde head actual
`git tag _tag_ _commit_` | asigna etiqueta a commit donde esta HEAD
`git describe _rama_` | muestra info de tag_commits_hash de rama o HEAD

## 4. Fuentes
Libro de git: <https://git-scm.com/book/en/v2>

[//]: # (referencias citadas)
[gitInicio]: https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository
[gitTutorial]: https://rogerdudler.github.io/git-guide/index.es.html
[gitInteractivo]: https://learngitbranching.js.org/?locale=es_ES
