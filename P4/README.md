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
$ git clone https://github.com/SyTW2019/E01.git
```
<p align="center">
  <img src="https://i.imgur.com/y5SI6AZ.png"/>
</p>

## Ejercicios de merge entre ramas
<p align="center">
  <img src="https://i.imgur.com/RfjtTWL.png"/>
</p>


## Pruebas con la aplicacion bitname
