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

Lo que nosotros tenemos que hacer ahora es configurar nginx para que pueda tratar las IPs que le llegan por el puerto 80 y enviar la solicitud al puerto de escucha del backend (que según nuestro esquema de la práctica anterior es 172.16.16.2). Para ello nos crearemso un fichero llamado *reverse-proxy.conf* ubicado en */etc/nginx/sites-available* que contenga:

<p align="center">
  <img src="https://i.imgur.com/Bru2MI5.png"/>
</p>

Con dicha configuración hacemos que escuche cualquier IP en el puerto 80 y redirija con proxy_pass a nuestra interfaz del backend que *comparte network* con el proxy.

Luego tenemos que desvincular el fichero al que por defecto nginx tiene vinculado. Nos dirigimos a la carpeta *sites-enabled* y dentro de esta ejecutamos la siguiente instrucción.
```
$ unlink default
```
Aunque borrarlo también serviría. Lo importante viene ahora para vincular nuestro nuevo fichero.
```
$ ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled
```
Y al tratar de obtener una lectura deberíamos tener esto:
<p align="center">
  <img src="https://i.imgur.com/xwT7zjm.png"/>
</p>

Ya podemos pasar a configurar el backend.

## (En el backend) Configuración NodeJS
La configuración del backend se podría decir que es la más sencilla de las 4 componentes que vamos a tratar de incluir en nuestro modelo a tres capas. Pues basta con instalar el NodeJS mediante la siguiente instrucción (asegurándose siempre de actualizar la máquina) y, en este caso usando cURL para extraer el script bash
```console
$ sudo apt update
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt install nodejs
```
Luego completamos la instalación con el gestor de paquetes de node *npm* (algo muy importante), pues así podremos instalar los *node_modules* del que disponga cada aplicación o proyecto.
```console
# apt install build-essential libssl-dev
```

Lo normal es que los paquetes deban ser instalados dentro de los proyectos, a menos que se especifiquen que sean globales "-g" en cuyo caso se instalarán en un directorio del que node se encargará de leer para cada aplicación que exista en esa máquina y no haya que instalarlos manualmente. Express es uno de ellos. Es el framework web más popular de Node, y es la librería subyacente para un gran número de otros frameworks web de Node populares. Proporciona mecanismos para utilidades HTTP. Así pues, es requerido instalarlo.
```
$ npm install express
```

Ya tenemos lo necesario para desplegar nuetsra aplicación, ahora lo suyo será ejecutar una. En este caso nos serviremos de un [ejemplo de un repositorio github](https://github.com/crguezl/express-start). Que si bien podemos descargarlo y subirlo  nuestra máquina si disponemos de un sistema de montajes como es *ssfh* para windows (un método más cómodo) lo normal es hacerlo por git clone, o incluso hay quien lo sube con scp. El caso es que hay múltiples maneras así que cada quien elige la suya. 

Una vez tengamos subida la aplicación en la dirección de nuestro home *($HOME/express-start)* nos dirigimos a su ubicación e instalamos sus paquetes
```
$ sudo npm i
```
Y auqneue s opcional, es recomendable instalar un módulo llamado *nodemon* que nos facilitará el tener que arrancar la máquina cada vez que se crashee, pues ésta consigue levantar la app escuchando los cambios en el fichero constantemente. 

Habrá que cambiar una serie de datos para que la aplicación pueda escucchar en la IP que nos interesa con el puerto 3000.


## (En la BDD) Configuración MongoDB
```console
$ sudo apt update
$ sudo echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.2 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
$ sudo apt-get update
$ sudo apt install mongodb-org -y
```

en */etc/systemd/system/mongodb.service*
(insertar imagen de configuración)
```
$ sudo systemctl enable mongodb
```

en */etc/mongod.conf*
(insertar imagen de configuración)
```
$ sudo systemctl start mongodb
$ mongo
```

Crearemos nuestra base de datos con usuario con permisos de lectura y escritura (nuestro usuario admin). Similar al usuario postgres que actua como super usuario del sistema gestor de postgresql.
```
> use admin
> db.createUser({user: "dbadmin", pwd: "secretpass", roles:[{role: "root", db: "admin"}]})
```

Paramos mongodb y lo volvemos a iniciar. Iniciamos sesión luego a la shell de mongo como usuarioa admin
```
$ mongo -u "dbadmin" -p "secretpass" --authenticationDatabase "admin"
```

Si no dio error todo salió perfecto. Crearemos ahora una base de datos que será la que se comunique con la aplicación del backend desde esta nuestra sesión con la shell identificasos como dbadmin en admin.
```
> use db01
switched to db db01
> db.createUser({user: "james", pwd: "pass", roles: ["readWrite"]})
Successfully added user: { "user" : "james", "roles" : [ "readWrite" ] }
```
Se habrá de recordar estos datos pues serán los que se van a usar en nuestra aplicación MEAN

## (Backend y BDD) Montaje NFS

