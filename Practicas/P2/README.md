# Práctica 2: Clonar la información de un sitio web

## Objetivos
Los objetivos concretos de esta segunda práctica son:
* Aprender a copiar archivos mediante `ssh`.
* Clonar contenido entre máquinas.
* Configurar `ssh` para acceder a máquinas remotas sin contraseña.
* Establecer tareas en `cron`.

## Comprobando SSH

Lo primero que haremos aquí será probar `ssh`, por lo que deberemos como primera medida comprobar las IPs de cada máquina que utilicemos. Para el caso que trato, dejo una camputura de pantalla para este hecho:

| Comprobar IPs | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

Hecho esto, crearemos un directorio con un archivo que será el que enviemos a la otra máquina. Para hacerlo lo hemos comprimido, pero lo que nos interesa realmente es enviarlo, así que dejo una camptura de pantalla con el uso de `ssh` para el envío:

| Comprobar IPs | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/envioArchivo-ssh.PNG) |

## Comprobando rsync

Este es otro método para poder mandar de un terminal a otro que especialemnte recomiendo para volúmenes de datos grandes.
La herramienta `rsync` nos permitirá copiar una estructura de directorios completa, además controlando que la información que contiene la copia siempre es idéntica a la original. Para evitar contratiempos en su uso, voy a utilizar en ambas cuentas **root**. Para activarlo usaré `sudo su`.
Una vez hecho esto, ejecutamos `rsync` pasándole como argumentos **-avz -e**:
+ a: se usa para que los archivos copiados conserven sus permisos y propiedades originales 
+ v: se usa para que se muestren por pantalla todas las operaciones que están siendo realizadas 
+ z: se usa para usar compresión 
+ e: se usa para indicar que vamos realizar la copia en otro ordenador 

En el caso que presento, se utilizará `ssh` y `ssh root@192.168.75.129:/var/spool /var/inventada` que se utiliza para conectarse mediante ssh a la máquina indicada. 
Usando el usuario root, copiará la carpeta “/var/spool/” y todo su contenido en la carpeta remota “/var/”.

Introducido esto, pedirá la contraseña de **root** y nos mostrará por pantalla las acciones realizadas. Finalmente comprobamos que los archivos copiados son exactos a los originales.



[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P1/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P3/README.md)
