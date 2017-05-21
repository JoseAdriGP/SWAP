# Práctica 5: Replicación de bases de datos de MySQL

## Objetivos
Los objetivos concretos de esta práctica son:
- Copiar archivos de copia de seguridad mediante ssh.
- Clonar manualmente BD entre máquinas.
- Configurar la estructura maestro-esclavo entre dos máquinas para realizar el clonado automático de la información.

## Crear un tar con ficheros locales y copiarlos en un equipo remoto
Los paso para hacer esto se describen en la [práctica 2](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/README.md). En caso de necesidad de repetir este proceso visitad la documentación de la práctica 2.

## Crear una BD e insertar datos
Para poder realizar sin problemas las tareas de replicación, se debe acceder a la base de datos con el usuario “root”, por lo que se requirá la contraseña para acceder con dicha cuenta. Para cambiar la contraseña fácilmente habrá que parar MySQL con `service mysql stop` y pornenla en marcha en modo seguro sin que cargue la tabla de privilegios con `mysqld_safe -–skip-grant-tables`.

| Reiniciando MySQL | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/01.PNG) |

Para poder continuar se necisita trabajar con otro terminal, pero al no ser esto posible utilizando VMware, simplemente se cambiará la ultima orden por `mysqld_safe -–skip-grant-tables &`.

| Reiniciando MySQL en caso de no poder iniciar un segundo terminal| 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/02.PNG) |

Ahora se podrá acceder como usuario **root** sin ser necesario introducir contraseña mediante la orden `mysql –u root`. Se seleccionará la base de datos que contiene los datos de los usuarios y ya se podrá cambiar la contraseña del usuario **root**. Solo faltaría actualizar los cambios realizados (`flush privileges`) y salir.

| Entradas con al orden `mysql –u root` | Uso de `flush privileges` y salida |
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/03.PNG) |![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/04.PNG) |

Como podemos ver en la imagenes anteriores, las ordenes usadas dentro de `mysql –u root` son:
- use mysql;
- update user set password=PASSWORD("laNuevaClaveSegura") where User='root';
- flush privileges;
- quit

La clave dada en este ejemplo es `laNuevaClaveSegura`, pero por supuesto esto está sujeto a modificación.

Hecho esto, se debe pasar a parar e iniciar MySQL. Ahora se buscará su PID con `ps aux | grep mysql`, matándolo con `kill -9 PID`.Con esto se evitan problemas para acceder con la nueva contraseña actualizada de **root**.

| Evitar porblemas para acceder como root | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/05.PNG) |

Como ahora el usuario “root” tiene contraseña, se accederá con el comando `mysql –u root –p` introduciendo la contraseña.
| Evitar porblemas para acceder como root | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/06.PNG) |

La creacíon de la base de datos se ralizará mediante comandos de SQL. Para aquellos que no sepán sobre SQL, consultad un tutorial o recuperad los apuntes de FBD.

| Creación de la BD | Insercion de datos | Uso de describe |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/07.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/08.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/09.PNG) |

## Replicar una BD MySQL con mysqldump
Para realizar este ejercicio habrá bloquear la base de datos principal para que no se pueda cambiar nada.

| BLoquear BD | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/10.PNG) |

La copia de la BD se realizará mediante la herramienta `mysqldump`,  introduciendo el comando `mysqldump peliculas -u root -p > /root/peliculas.sql`. También se pedirá la contraseña de **root**.

| Realizar copia de la BD | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/11.PNG) |

Para desbloquear la base de datos principal y volver a trabajar con ella normalmente se debe introducir `UNLOCK TABLES`.

| Desbloquear BD | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/12.PNG) |




## Replicación de BD mediante una configuración maestro-esclavo

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P4/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Ejercicios/T1.md)

