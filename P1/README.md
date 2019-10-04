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
Para el proxy seleccionamos un DOC1(nic1) y un DOCINT1(nic2)
Para el backend seleccionamos un DOCINT1(nic2) y un DOCINT(nic1)
Para el backend únicamente agregamos una interfaz, pues sólo estara conectado al backend, ésta será DOCINT2(nic1)

Esto se hace para el DOCINT de cada máquina quede emparejado sin aportar ningún error.
