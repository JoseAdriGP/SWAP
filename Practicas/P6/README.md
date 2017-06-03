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
| [Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-01.PNG ) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-02.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-03.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/Images/P6-04.PNG) |

Como se puede apreciar, por defecto crea un disco duro con el mismo espacio y aunque se puede cambiar, por defecto marcará el tipo de disco `SCSI`.

## Configuración del RAID por sofware


[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Ejercicios/T1.md)

