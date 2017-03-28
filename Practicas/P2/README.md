# Práctica 2: Clonar la información de un sitio web

## Objetivos
Los objetivos concretos de esta segunda práctica son:
* Aprender a copiar archivos mediante `ssh`.
* Clonar contenido entre máquinas.
* Configurar `ssh` para acceder a máquinas remotas sin contraseña.
* Establecer tareas en `cron`.

## Comprobando SSH

Lo primero que haremos aquí será probar `ssh`, por lo que deberemos como primera medida comprobar las IPs de cada máquina que utilicemos. Para el caso que trato, dejo una captura de pantalla para este hecho:

| Comprobar IPs | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

Hecho esto, crearemos un directorio con un archivo que será el que enviemos a la otra máquina. Para hacerlo lo hemos comprimido, pero lo que nos interesa realmente es enviarlo, así que dejo una captura de pantalla con el uso de `ssh` para el envío:

| Enviar archivo mediante ssh | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/envioArchivo-ssh.PNG) |

## Uso de rsync, clonar contenido de la máquina

Este es otro método para poder mandar de un terminal a otro que especialmente recomiendo para volúmenes de datos grandes.
La herramienta `rsync` nos permitirá copiar una estructura de directorios completa, además controlando que la información que contiene la copia siempre es idéntica a la original. Para evitar contratiempos en su uso, voy a utilizar en ambas cuentas **root**. Para activarlo usaré `sudo su`.
Una vez hecho esto, ejecutamos `rsync` pasándole como argumentos **-avz -e**:
+ a: se usa para que los archivos copiados conserven sus permisos y propiedades originales 
+ v: se usa para que se muestren por pantalla todas las operaciones que están siendo realizadas 
+ z: se usa para usar compresión 
+ e: se usa para indicar que vamos realizar la copia en otro ordenador 

En el caso que presento, se utilizará `ssh` y `ssh root@192.168.75.129:/var/inventada /var/` que se utiliza para conectarse mediante ssh a la máquina indicada. 
Usando el usuario root, copiará la carpeta “/var/inventada” y todo su contenido en la carpeta remota “/var/”.

Introducido esto, pedirá la contraseña de **root** y nos mostrará por pantalla las acciones realizadas. Finalmente comprobamos que los archivos copiados son exactos a los originales.

## Facilitar el uso de la herramienta. Claves públicas y privadas
Para no tener que introducir la contraseña del usuario cada vez que queramos hacer una copia de seguridad, se utilizará la autenticación mediante par de claves pública-privada. 
Lo primero será generar la clave en el ordenador desde el que realizaremos la conexión. Generar la clave se hace mediante `ssh-keygen –t dsa`, que generará una clave DSA. No se debe introducir ninguna contraseña cuando la solicite, ya que el objetivo es no tener que introducir contraseña.

| Generación de archivo DSA | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/generaci%C3%B3nDSA.PNG) |

Al no cambiar la ruta en la que se generará el fichero con la clave, debe encontrarse en `/root/.ssh/id_dsa`. 
Copiaremos este archivo ordenador del que queremos copiar la información sin tener que introducir la contraseña. 
Desde la máquina en la que copiar usaremos la orden `ssh-copy-id –i .ssh/id_da.pub root@192.168.75.129`, con lo que estaremos copiando en el ordenador principal la clave que nos permitirá copiar en nuestro ordenador los archivos con `rsync` mediante ssh sin tener que introducir la contraseña en cada ocasión. 
En el archivo de las claves del ordenador principal (/root/.ssh/authorized_keys), hay al final una entrada que indica que está permitido la conexión mediante ssh al usuario “root” desde la máquina "muramasa".

Para comprobar el funcionamiento de lo realizado anteriormente, si se realiza una conexión mediante ssh al ordenador principal desde el ordenador secundario podremos ejecutar una orden cualquiera, como puede ser `uname –a`.

## Nuevo uso de rsync. Prueba de la anterior configuración.

Se va a volver a realizar la copia del directorio **“/var/inventado/”** con `rsync`, pero esta vez pasándole los argumentos:
+ `--delete`: los archivos que hayan sido eliminados en la máquina origen, también serán eliminados en la máquina de copias en caso de existir
+ `--exclude`: para que no se copien los archivos indicados

> Un ejemplo de usuo sería `rsync -avz --delete --exclude =**/error --exclude =**/pictures/readme.txt -e "ssh -l root" root@192.168.75.129 /var/inventado /var/ `

En `ssh` también se debe indicar que la conexión se realizará como **“root”** mediante: -e `ssh –l root`. 
Si se realiza correctamente la creación y copiado de las claves, la ejecución de `rsync` no pedirá contraseña.

Para automatizar la copia con `rsync` mediante `cron`, se puede crear un script que realice la operación de copiado que acabamos de ver con `rsync`, y una vez creado el script, se le darán permisos de ejecución para solo nuestro usuario: `chmod 744 scriptActualizar.sh`. 

Para finalizar faltaría es añadir una nueva tarea al **“crontab”** (fichero de configuración de `cron` en **“/etc/crontab”**), para que cada hora se realice la copia de seguridad en nuestra máquina replicante, así que en este caso deberemos añadir: `0 *      * * *  root    /root/scriptActualizar.sh`.

Aquí se indica que en el minuto **“0”** de cada hora, de cada día del mes, de cada mes, de cada día de la semana, el usuario **“root”** ejecute el script **“/root/scriptActualizar.sh”**, que es el script que acabamos de crear para que realice la copia del contenido actualizado del directorio **“/var/inventado/”** de la máquina principal a la máquina replicante.


[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P1/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P3/README.md)
