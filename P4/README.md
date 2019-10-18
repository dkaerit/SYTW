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
<p align="center">
  <img src="https://i.gyazo.com/7710a99b95a032da85bdd2c97dbb1055.png"/>
</p>

<p align="center">
  <img src="https://i.gyazo.com/8f88e35e953aa98aeeb2c2a2474e68b3.png"/>
</p>

## Pruebas con la aplicacion bitname
