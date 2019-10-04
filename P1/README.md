```
SISTEMAS Y TECNOLOGÍAS WEB — DIEGO VÁZQUEZ CAMPOS
GRADO EN INGENIERÍA INFORMÁTICA — ULL
```
# Laboratorio virtual en IaaS de la ULLTarea
El objetivo de la práctica es dejar preparado una serie de conexiones entre tres servidores (o máquinas virtales en el caso real) para simular la interconexión de un proxy con un servidor que hará de backend, y éste a su vez estará conectado a otra máquina que hará de la supuesta base de datos. 

Para ello se nos ha ofrecido un rango de IP que será el que usaremos para establecer las distintas subredes que harán posible dicha conexión. En este caso trabajaremos con las IP comprendidas entre 172.16.16.X y 172.16.17.X con máscara 255.255.255.0

Una visual de lo que queremos conseguir es lo siguiente.


<p align="center">
  <img src="https://github.com/monnizou/SYTW/blob/master/P1/imgs/diagram.png"/>
</p>

## Creando las máquinas virtuales
Nos dirigimos a la página del iaas de la universidad que es donde crearemos las tres máquinas. Un aso bastante sencillo pues se ofrece una plantilla de Debian y lo único que hay que hacer es especificar las interfaces de cada una de ellas.

```
Show advanced options -> General
```
- Para el proxy seleccionamos un DOC1(nic1) y un DOCINT1(nic2)
- Para el backend seleccionamos un DOCINT1(nic2) y un DOCINT(nic1)
- Para el backend únicamente agregamos una interfaz, pues sólo estara conectado al backend, ésta será DOCINT2(nic1)

*Esto se hace para el DOCINT de cada máquina quede emparejado sin aportar ningún error.*

Ahora entramos en cada una de las máquinas y configuraremos sus interfaces. Para saber el estado de las mismas usamos el comando
```console
$ ip a
```
<p align="center">
  <img src="https://github.com/monnizou/SYTW/blob/master/P1/imgs/captu1.png"/>
</p>
Aquí vemos que hay 3 interfaces habilitadas, el loopback, que no lo tocaremos el ens3 y el ens6 (en la captura se ven que están activas porque ya las levanté al realizar la práctica, pero inicialmente estará una de ellas baja sin IP asignada). 

Otro dato importante es que no sabemos a qué interfaz se refiere cada una de las que ahí muestra pues nosotros teníamos entendido su nombre por nic1, nic2, etc. Lo que podemos hacer es mirar la mac de cada una de esas interfaces y hacerlas coincidir para saber cual es cual. 

Si vamos a la página del iaas sección "/mismaquinas" se podrá acceder a un listado de todas las máquinas habilitadas y datos como la mac y la IP de cada una de esas interfaces.
<p align="center">
  <img src="https://github.com/monnizou/SYTW/blob/master/P1/imgs/screen2.png"/>
</p>

En realidad no tendría que venir marcado ya directamente la relación nic/ens, pero en este caso ya se ha tomado la molestia de anotar, y así tenerlo siempre presente, a quién corresponde qué interfaz con la ayuda de tampermonkey, que no tiene que ver con esta práctica, pero con un script se puede modificar la vista del navegador para añadir más datos al hacer coincidir las mac que vemos con el comando ya mencionado para ver la información de las interfaces en la bash de debian.

## Editando los ficheros de configuración de interfaces
Antes de entrar en cuestión, algo que resulta importante para no despistarnos en las distintas sesiones es cambiar el hostname de las máquinas y así saber onde uno se encuentra en cada momento. El comando que usaremos para ello es:
```console
$ sudo hostnamectl set-hostname <nuevo nombre>
```
Luego, haciendo esto en las tres máquinas se debería tener tres sesiones de tipo parecidas a usuario@sytw_backend, usuario@sytw_bdd, usuario@sytw_proxy que son los nombres que en esta práctica se les dio.

Ahora ya podemos centrarnos en modificar los ficheros de configuración. Entraremos en modo superusuario.
```console
$ sudo su
```
La contraseña será la misma que se usó al cambiar la password en el primer inicio de sesión. Es decir la misma que "usuario", en este caso.

Editaremos con nano o con vi el fichero interfaces ubicado en /etc/network. Aunque sería una mayor comodidas montar por remoto el sistema de ficheros con ayuda de servicios como sshfs y usar nuestro editor de texto favorito, no obstante, la falta de permisos no nos dejarían editar este tipo de ficheros.
```console
$ nano /etc/network/interfaces
```
Y debería modificarse el fichero hasta tener algo como esto

<p align="center">
  <img src="https://github.com/monnizou/SYTW/blob/master/P1/imgs/proxy_screen.png"/>
</p>

El dhcp que viene por defecto no lo tocamos, pero añadimos una entrada para el ens3 con la información dispuesta en el esquema del principio. Repetimos la misma mecánica en el resto de máquinas.


