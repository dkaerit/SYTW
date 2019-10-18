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
<p align="center">
  <img src="https://i.imgur.com/obRxkvt.png"/>
</p>
<p align="center">
  <img src="https://i.imgur.com/5J9l3LL.png"/>
</p>

## Clonado de la MV Backend
## Configurar el proxy como sistema de balanceo de carga

<p align="center">
  <img src="https://i.imgur.com/Y3qilVW.png"/>
</p>
