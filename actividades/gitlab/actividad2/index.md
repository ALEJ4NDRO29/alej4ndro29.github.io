# Actividad ramificaciones
A continuación crearemos un repositorio siguendo el sistema de ramificaciones propuesto por Vincent Driessen (Git flow).


## Crear un repositorio con las ramas de master y develop.
```
git checkout -b master
git checkout -b develop
```


## Generar diferentes features con pequeños cambios en el proyecto. Algunas de las features deben modificar un mismo fichero.

```
git checkout develop
git checkout -b feature1
git add .
git commit -m "feature 1"
```

## Crear una nueva release con todas las features creadas.
```
git checkout develop
git merge feature1
git merge feature2
git checkout master
git merge develop
```
Solucionamos los posibles conflictos.

## Simular que se ha detectado un error en la release y solucionarlo.
```
git checkout feature1
git checkout -b fixError
```
` Realizamos los cambios correspondientes. `

```
git add .
git commit -m "error fixed"
git checkout feature1
git merge fixError
```

## Publicar la release en producción y esta debe quedar etiquetada.

```
git checkout develop
git merge feature1
git checkout master
git merge develop
git tag -a v1.1 -m "new version"
git push --all origin
```
## Simular la existencia de un error en producción y solucionarlo por medio de un hotfix.
```
git checkout master
git checkout -b hotfix1
```
` Realizamos los cambios correspondientes. `
```
git add .
git commit -m "error resuelto"
git checkout master
git merge hotfix1
```