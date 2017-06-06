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

A partir de aquí queda pasar el archivo `.sql` que se ha generado con anterioridad, en el caso de este ejemplo `peliculas.sql`. Para hacer esto, se puede solicitar desde la máquina que recibirá la copia o enviarla desde aquella en la que se creo la bd.

| Enviar archivos de m1 a m2 | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/13.PNG) |

A continuacion vemos la comprobación de la recepción de los archivos:

| Comprobación de que el archivo se encuentra en el directorio | Uso de cat para comprobar el contenido del documento |
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/14.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/15.PNG) |

Con esta parte realizada, se deberá crear una bd con el nombre peliculas en mysql en la máquina que acaba de recibir el archivo, ya que este posee la información, pero no las ordenes para crear una nueva bd. Como el proceso es el mismo que en la anterior máquina, simplemente se debe repetir los pasos descritos en la creación omitiendo aquellos en los insertamos información en la bd.

Una vez creada la BD, se pasa a introducir los datos del archivo mediante la orden `mysql -u root -p peliculas < /home/pedro/peliculas.sql`.

| Inserción de datos en la BD | Conprobación de los datos de la BD |
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/16.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/17.PNG) |

## Replicación de BD mediante una configuración maestro-esclavo

Para no tener que replicar manualmente siempre que se produzcan cambios en la base de datos, se puede configurar el servidor de respaldo para que funcione como servidor esclavo, el cuál se actualizará siempre que se produzca un cambio en el servidor principal. El servidor principal será el que funciona como maestro. 

Para empezar hay que editar el archivo de configuración `/etc/mysql/my.cnf` en el servidor maestro. Los parámetros más importantes son:
- `server-id` -> indica el identificador de nuestro servidor maestro.  Valor `1` 
- `log_bin` -> indicar el archivo binario donde se almacenarán las transacciones a realizar para mantener siempre el servidor esclavo actualizado. Valor `mysql-bin`. 

Para saber con certeza que los datos están siendo constantemente actualizados, se debe indicar en `sync_binlog=1` que el registro binario se sincronice con el disco tras cada escritura que se produzca en él. El resto de parámetros que se muestran son para aspectos de los archivos de registro que también se pueden incluir. En este mismo archivo de configuración se debe comentar la línea `bind-address=127.0.0.1` para que el sistema escuche el resto de equipos de la red que no sean el propio host. Para que la configuración se haga efectiva, se debe reiniciar **“mysql”**.

| Comentar linea `bind-address=127.0.0.1` | Editar parámetros 1 |  Editar parámetros 2 |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/18.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/19.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/19B.PNG) |

Habrá crear el usuario que va a usar el servidor esclavo para conectarse al servidor maestro y actualizarse, por lo que en el **servidor maestro** se creará el usuario con permisos de replicación (replication slave) `replislave` que se podrá conectar desde la dirección  IP  192.168.75.129  (la  dirección  IP  del  servidor  esclavo),  identificándose  con  la contraseña `pass`. Se hará efgectivo recargando la tabla de permisos con `flush privileges`.

| Orden en la base de datos | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/20.PNG) |

Se necesitará una copia actual de la base de datos en el servidor maestro para aplicar en el servidor esclavo. El proceso se hará como se vio en la copia manual: 
#### En el servidor maestro
- Bloqueo de las tablas de la base de datos: `flush tables with read lock;` 
- Salir de MySQL: quit
- Crear la copia: `mysqldump peliculas –u root –p > /root/contactos.sql`
- Entrar en MySQL para desbloquear las tablas: `unlock tables;`
- Salimos para transferir la copia al servidor esclavo: `scp peliculas.sql pedro@192.168.75.129:/home/pedro`
#### En el servidor esclavo
- Accedemos a MySQL para crear la base de datos a importar: `create database peliculas;`
- Salir de MySQL: `quit`
- Aplicar la copia sobre dicha base de datos: `mysql –u root –p peliculas < peliculas.sql;`

A continuación se editará el archivo de configuración de MySQL en el servidor esclavo de la misma forma que el maestro. Se especificará un identificador único para este servidor, y puesto que se ha identificado el servidor maestro como `1`, el servidor esclavo recibirá como valor `2`.

| Modificación del fichero `/etc/mysql/my.cnf` en el servidor esclavo | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/21.PNG) |

Habrá que comprobar si la configuración hecha hasta el momento funciona, por que lo que se reniciará primero MySQL en el servidor esclavo, haciendo posteriormente una conexión remota al maestro accediendo con el usuario creado anteriormente mediante la orden `mysql -h 192.168.75.131 -u repislave -p'pass'`
Parámetros:
- Nombre de usuario: `replislave`
- Contraseña `pass`

| Conexión desde el servidor esclavo | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/22.PNG) |

Para poder empezar con la sincronización, se volverá al maestro para bloquear de nuevo las tablas (`flush tables with read lock;`). A partir de aquí habrá que buscar la información sobre el archivo binario que almacena las transacciones al que se hace referencia en la configuración del servidor maestro (`show master status;`). Interesa conocer el nombre de dicho archivo y la posición para que, a partir de ahí, sea la información que utiliza el maestro para actualizarse.

| Uso de `show master status;` | 
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/23.PNG) |

Se debe volvemos ahora al servidor esclavo donde se introducirá la información necesaria para que se pueda conectar al maestro con la consulta:

```
change master to 
master_host=’192.168.75.131’, 
master_user=’replislave’, 
master_password=’pass’, 
master_log_file=’bin.000001’, 
master_log_pos=107,
master_port3306;
```

Los dos últimos valores son los obtenido del estado del servidor maestro. Ahora habrá que desbloquear las tablas en el servidor maestro (`unlock tables;`) e iniciar el servidor esclavo para que funcione como esclavo (`start slave;`).

| Cambios de maestro en el esclavo | Desbloquear las tablas en el maestro |
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/24.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/25.PNG) |

Ahora se comprobará que el **“esclavo”** está funcionando, para lo que se tendrá que mostrar su estado. Como la salida generada no puede ser mostrada en una sola pantalla, se redirigira su salida a un archivo para visualizarla más fácilmente.

| Orden para extraer la información del estado |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/26.PNG) |

Siendo esta la salida obtenida con todos los valores actuales  de configuración del servidor esclavo, entre otros se ve la dirección del servidor maestro (**192.168.75.131**) o el usuario que usa para acceder al servidor maestro (**replislave**):

| nano 1 | nano 2 | nano 3 |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/27A.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/27B.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/27C.PNG) |

Ya con toda la configuración realizada, solo faltará probar que la sincronización automática funciona correctamente. En el servidor maestro se realiza una consulta de selección de todos los datos en la tabla existente (`select * from datos`). Ahora mediante una consulta de inserción se introduce un nuevo valor en la base de datos. Hehco esto se retornará al esclavo y se consulta con una selección. Si se muestra el nuevo de registro introducido desde el servidor maestro la sincronización automática estará funcionando.

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P4/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P6/README.md)

