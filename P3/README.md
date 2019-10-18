```
SISTEMAS Y TECNOLOGÍAS WEB — DIEGO VÁZQUEZ CAMPOS
GRADO EN INGENIERÍA INFORMÁTICA — ULL
```

# Ejercicios con el repositorio del proyecto
El objetivo de esta práctica es completar el despliegue de la aplicación MEAN en cuayo caso de no haberla tenido lista en la práctica 2, y luego de ellos, clonar la máquina correspondiente al backend para crear una estrategia horizontal que haga de balanceo de carga. Con ello, necesitaremos configurar el nginx para que nos dote de ese servicio.
## Desplegar en el backend una aplicacion MEAN de prueba
Lo primero de todo será descargar nuestra aplicación bitnami y cargarla con el método que se prefiera (git clone, usando puntos de ruta de windows con sshfs, o trasnfiriendo con scp). Tendremos que tocar el script de ejecución para que el uid definido allí sea el mismo que el de nuestra máquina, lo que se traduce en sustituir todas las veces que se repita la palabra "bitnami".
<p align="center">
  <img src="https://i.imgur.com/3AzrPWn.png"/>
</p>
Debemos disponer de un fichero *.deployment.env* (fichero de configuración de variables de entorno) ubicado en la carpeta */root*, para ofrecer la información requerida en el script. Queda bastante claro que debemos hacerlo como usuario root para que sea posible.

Dicho fichero debe contener la siguiente información basándonos en la guía del funcionamiento interno de la propia aplicación explicado en su repositorio.

<p align="center">
  <img src="https://i.imgur.com/obRxkvt.png"/>
</p>

En *DATA_FOLDER* se pondrça la ruta de de nuestro montaje hecho desde la base de datos, y en *APP_FOLDER* la ruta hasta nuestra aplicación MEAN. el resto de constantes son bastante explícitas y no necesitan explicación, pues el PATH aunque estaría bien ponerlo exactamente como en la captura no se va a usar al igual que las opciones de conexión.

Una vez conseguido esto iniciamos los servicios necesarios y con la aplicación en marcha entraremos como ya conocemos desde el navegador, y listo.

```console
$ sudo ./run.sh start
```

<p align="center">
  <img src="https://i.imgur.com/EUXNVzX.png"/>
</p>

## Clonado de la MV Backend
Antes de nada, por seguridad paramos la aplicación
```console
$ sudo ./run.sh stop
```
Con esto, desde nuestro panel de administración (IaaS) donde visualizamos las máquinas virtuales, hacemos shutdown en la máquina objetivo y con click derecho seleccionamos *Clone MV*

Debemos configuar (o más bien, modificar) la interfaces para esta nueva máquina atendiendo a los *network* que establecimos en la práctica 1. Es decir, si nuestros network eran:

*172.16.16.0* Para las conexiones entre backend y proxy.
*172.16.17.0* Para las conexiones entre backend y bdd.

Podemos asignarle *172.16.16.3* y *172.16.17.3* a los nic (ens o DOCINT) que corresponde según vimos en la primera práctica.

Levantaremos de nuevo las máquinas y nos moveremos al proxy para configurar el servicio de balance de carga con nginx.

## Configurar el proxy como sistema de balanceo de carga
Habrá que crear un fichero para disponer de dicho balanceador. Como ya conocemos, usaremos nuestro editor de texto preferido con permisos de superusuario.
```
sudo nano /etc/nginx/conf.d/balancer.conf
```
<p align="center">
  <img src="https://i.imgur.com/Y3qilVW.png"/>
</p>
Tras reiniciar el servicio podremos comproar que todo esté correcto mirando los logs del balanceador */var/log/nginx/access.log* y si se levantó con pm2 *root/.pm2/logs*

Luego podemos verificar, cómo nuestro acceso a la página es distribuido en las distintas máquinas backend monitorizando las nuevas sesiones.
