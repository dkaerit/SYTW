```
SISTEMAS Y TECNOLOGÍAS WEB — DIEGO VÁZQUEZ CAMPOS
GRADO EN INGENIERÍA INFORMÁTICA — ULL
```

# Ejercicios con el repositorio del proyecto
## Clonar el repositorio
Generamos clave privada y clave pública en nuestro directorio *~/.ssh* (lo crearemos si no lo tenemos), o podemos usar la clave pública que ya hayamos creado previamente.
```console
$ ssh-keygen -t rsa
```
Copiamos y pegamos la clave publica en nuestro gestor declaves de github, en *Settings*.
<p align="center">
  <img src="https://i.imgur.com/jp9RxGG.png?1"/>
</p>

Comprobamos que podemos iniciar sesión correctamente a github en remoto.

```console
$ ssh git@github.com
```
Clonamos nuestro repo en el directorio que queramos.

```console
$ git clone git@github.com:SyTW2019/E01.git
```
<p align="center">
  <img src="https://i.gyazo.com/483fe5732abb03d78a903d0f88608c25.png"/>
</p>


## Ejercicios de merge entre ramas
Lo que viene a contnuación es una practica común para un caso en el que se quiere crear, por ejemplo un fichero, pero se quiere mantener actualizado el branch de lo que hay en el master.
```
$ git brach monnizou
$ git checkout monnizou
(editando)
$ git add *
$ git commit -m "aquí un resumen del cambio realizado"
$ git merge master
$ git checkout master
$ git merge monnizou
```
Lo que se está haciendo es chear el branch monnizou donde se trabajará de forma que no se afecte al master, luego nos movemos al branch, editamos lo que queramos, preparamos los ficheros y hacemos un commit (actualizamos los cambios en el branch) unimos lo que haya en el master en ese momento a nuestros cambios, para que nada se pierda si se ha hecho algo nuevo en el master, nos movemos al master y unimos lo que nosotros hayamos hecho el branch, hacemos un push. y listo.
<p align="center">
  <img src="https://i.gyazo.com/7710a99b95a032da85bdd2c97dbb1055.png"/>
</p>

<p align="center">
  <img src="https://i.gyazo.com/8f88e35e953aa98aeeb2c2a2474e68b3.png"/>
</p>

## Pruebas con la aplicacion bitname
Se puede ver que la aplicación bitnami ya ha sido copiada por los compañeros de equipo tras tener todos esos ficheros correspondientes a la aplicación desde que hacemos el clone. Sin embargo podíamos hacerlo aunque se sobre escriba. Básicamente sería como vimos, nos clonamos la aplicación en nuestro branch y aplicamos los cambios con el método de trabajo git (teniendo cuidado de mantenernos actualizados del master) y hacemos un push de los ficheros incorporados. 

<p align="center">
  <img src="https://i.gyazo.com/acd9a983565e910f11885934300aeb5f.png"/>
</p>
