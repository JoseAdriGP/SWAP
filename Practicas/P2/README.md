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

| Enviar archivo mediante ssh | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/envioArchivo-ssh.PNG) |

## Uso de rsync, clonar contenido de la máquina

Este es otro método para poder mandar de un terminal a otro que especialemnte recomiendo para volúmenes de datos grandes.
La herramienta `rsync` nos permitirá copiar una estructura de directorios completa, además controlando que la información que contiene la copia siempre es idéntica a la original. Para evitar contratiempos en su uso, voy a utilizar en ambas cuentas **root**. Para activarlo usaré `sudo su`.
Una vez hecho esto, ejecutamos `rsync` pasándole como argumentos **-avz -e**:
+ a: se usa para que los archivos copiados conserven sus permisos y propiedades originales 
+ v: se usa para que se muestren por pantalla todas las operaciones que están siendo realizadas 
+ z: se usa para usar compresión 
+ e: se usa para indicar que vamos realizar la copia en otro ordenador 

En el caso que presento, se utilizará `ssh` y `ssh root@192.168.75.129:/var/inventada /var/` que se utiliza para conectarse mediante ssh a la máquina indicada. 
Usando el usuario root, copiará la carpeta “/var/inventada” y todo su contenido en la carpeta remota “/var/”.

Introducido esto, pedirá la contraseña de **root** y nos mostrará por pantalla las acciones realizadas. Finalmente comprobamos que los archivos copiados son exactos a los originales.

**Problemas a la hora de introducir la contraseña**

## Facilitrar el uso de la herramienta. Claves públicas y pribadas
Para no tener que introducir la contraseña del usuario cada vez que queramos hacer una copia de seguridad, se utilizará la autentificación mediante par de claves pública-privada. 
Lo primero será generar la clave en el ordenador desde el que realizaremos la conexión. Generar la clave se hace mediante `ssh-keygen –t dsa`, que generará una clave DSA. No se debe introducir ninguna contraseña cuando la solicite, ya que el objetivo es no tener que introducir contraseña.

| Generación de archivo DSA | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/generaci%C3%B3nDSA.PNG) |

Al no cambiar la ruta en la que se generara el fichero con la clave, debe encontrarse en `/root/.ssh/id_dsa`. 
Copiaremos este archivo ordenador del que queremos copiar la información sin tener que introducir la contraseña. 
Desde la máquina en la que copiar usaremos la orden `ssh-copy-id –i .ssh/id_da.pub root@192.168.75.129`, con lo que estaremos copiando en el ordenador principal la clave que nos permitirá copiar en nuestro ordenador los archivos con `rsync` mediante ssh sin tener que introducir la contraseña en cada ocasión. 
En el archivo de las claves del ordenador principal (/root/.ssh/authorized_keys), hay al final una entrada que indica que está permitido la conexión mediante ssh al usuario “root” desde la maquina "muramasa".

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P1/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P3/README.md)
