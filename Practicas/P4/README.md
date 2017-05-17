# Práctica 4: Comprobar el rendimiento de servidores web

## Instalación de SSL

Se procederá a la instalación de un *certificado SSL autofirmado*, que a pesar de no ser recomendable, será más que suficiente para la realización de la práctica.

En cualquiera de las dos máquinas con servidor web, se debe activar el módulo SSL en apache y crear el certificado autofirmado. Durante su creación, pedirá una serie de datos que habrá que rellenar.
> Recuerdo a aquellos que utilicen estas prácticas como guía que debemos tener apache funcionando. En caso contrario, se debe utilizar la orden *sudo service apache2 restart*
Las ordenes que deberemos seguir son:
- sudo a2enmod ssl
- sudo service apache2 restart
- sudo mkdir /etc/apache2/ssl
- sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P3/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/README.md)
