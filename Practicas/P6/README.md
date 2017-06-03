# Práctica 6. Discos en RAID

## Objetivos
Los objetivos concretos de esta práctica son:
- Configurar dos discos en RAID 1. Los discos se añadirán a un sistema ya instalado y funcionando, de forma que en total tendremos tres discos (el sistema operativo estará ya instalado en /dev/sda y añadiremos dos discos más, que serán el /dev/sdb y el /dev/sdc, para configurar el dispositivo de almacenamiento RAID en estos dos discos nuevos de igual tamaño).
- Hacer pruebas de retirar y añadir un disco “en caliente”, y comprobar que el RAID sigue funcionando correctamente.

## Preparación de la práctica. Creación del segudno disco en la máquina virtual

Este paso será explicado solo para VMware, pero es un paso realmente sencillo porque lo que encontrarlo para cuanlquier otro tipo de máquina virtual no será un problema. 
Para empezar se debe seleccionar la máquina a utilizar para para crear en ella un segundo discoduro. Para ello, se selecciona y se presiona en el botón `Edit virtual machine settings` que aparecerá al seleccionarla. A partir de aquí, solo habra que presionmar en el boton `add`, seleccionar `Hard Disk` y seguir los pasos 

| Botón `Edit virtual machine settings` | Seleccionar `Hard Disk` | Selección de `SCSI` | Nuevo disco añadido | 
| :-------------: | :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-01.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-02.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-03.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-04.PNG) |

Como se puede apreciar, por defecto crea un disco duro con el mismo espacio y aunque se puede cambiar, por defecto marcará el tipo de disco `SCSI`.

## Configuración del RAID por sofware

PAra hacer la configuración habrá que instalar *mdadm* mediante e comando `sudo apt-get install mdadm`, y mediante `fdisk -l` podremos saber cómo identificar los discos.

| Información mostrada por `fdisk` 1 | Información mostrada por `fdisk` 2 | 
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-05A.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-05B.PNG) |

Como se muestra en la imagen, la orden debe ejecutarse con `sudo`. En caso contrario **no mostrará** la información requerida. Por otro lado, el disco principal se sitúa y denomina como `/dev/sda`, frente a los secundarios añadidos en `/dev/sdb` y `/dev/sdc`.

En mi caso aparece esa información. Podemos apreciar que indica los 2 discos añadidos de tamaño 20GB aprox. Al disco principal lo sitúa y denomina /dev/sda y los secundarios añadidos en /dev/sdb y /dev/sdc.

Con la información obtenida se puede confirar ya el RAID 1 utilizando los dos discos añadidos para realizar el montaje. Con el comando `mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc` se indica que se haga en /dev/md0, nivel RAID 1 y número de dispositivos 2.

| Configuración `mdadm` |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-06.PNG) |

Si no ha ocurrido ningún problema se podra continuar dando formato al RAID mediante `mkfs /dev/md0` y montando el raid en un directorio exsistente mediante `mount /dev/md0 /datos` (en el caso presentado el directorio será datos). Es importante recordar que estos pasos deben realizarse con sudo o desde el usuario root.

Para comprobar el estado de la configuración se utilizará el comando siguiente `mdadm --detail /dev/md0`.

| Estado de la configuración mediante `mdadm --detail /dev/md0` |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-07.PNG) |

El último paso será configurar el servidor para que al arrancar monte el dispositivo RAID configurado en la carpeta indicada, pero antes de nada, mediante la orden `blkid` se comprobará la UUID del RAID.

| `blkid` |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-08.PNG) |

Habra que situarse en al fichero `/etc/fstab` para añadir la línea **UUID=d0b669c2-4c96-4407-b94b-e8409dee14fc /hopme/pedro/datos ext2 default 0 0**.

| Añadir la línea en `/etc/fstab` |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-09.PNG) |

Para comprobar que todo el proceso se ha realizado de forma correcta, se podrá reiniciar el sistema y comprobar si está montado el RAID en la carpeta `/hopme/pedro/datos` introduciendo el comando `mount`.

| Comprobación de la configuración mediante `mount` |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-09.PNG) |

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Ejercicios/T1.md)

