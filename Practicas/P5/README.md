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
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/Images/01.PNG) |

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P4/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Ejercicios/T1.md)

