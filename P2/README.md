```
SISTEMAS Y TECNOLOGÍAS WEB — DIEGO VÁZQUEZ CAMPOS
GRADO EN INGENIERÍA INFORMÁTICA — ULL
```
# Despliegue de una aplicación MEAN en el IaaS de la ULL
El objetivo de esta práctica es instalar los componentens necesarios y ejecutarlos para poder ejecutar una aplicación MEAN en nuestro backend de forma que esté protegida por nuestro proxy con la redirección adecuada.

## (En el proxy) Configuración NGINX
El servicio NGINX será nuestro proxy que filtrará las IP y reenviará las peticiones al backend para que podamos visualizar la aplicación express desde la interfaz DHCP del proxy (en nuestro caso 10.6.128.227) algo que se vio en la práctica anterior.

Lo primero será instalar NGINX (actualizamos previamente para evitar errores)
```console
$ sudo apt update
$ sudo apt install nginx
```
Luego podemos comprobar que el servicio corre perfectamente
```console
$ curl -I 127.0.0.1
```
Es importante conocer cómo manejar el servicio para (en orden de comandos) pararlo, levantarlo o reiniciarlo, volver a cargarlo despues de haber realizado algunos cambios, evitar que se inicie en el arranque o volver a habilitarlo:
```console
$ sudo systemctl stop nginx
$ sudo systemctl start nginx
$ sudo systemctl restart nginx
$ sudo systemctl reload nginx
$ sudo systemctl disable nginx
$ sudo systemctl enable nginx
```

## (En el backend) Configuración NodeJS

## (En la BDD) Configuración MongoDB

## (Backend y BDD) Montaje NFS
