# Práctica 4: Comprobar el rendimiento de servidores web

## Instalación de SSL

Se procederá a la instalación de un *certificado SSL autofirmado*, que a pesar de no ser recomendable, será más que suficiente para la realización de la práctica.

En cualquiera de las dos máquinas con servidor web, se debe activar el módulo SSL en apache y crear el certificado autofirmado. Durante su creación, pedirá una serie de datos que habrá que rellenar.
> Recuerdo a aquellos que utilicen estas prácticas como guía que debemos tener apache funcionando. En caso contrario, se debe utilizar la orden *sudo service apache2 restart*.

Las ordenes que deberemos seguir son:
- sudo a2enmod ssl
- sudo service apache2 restart
- sudo mkdir /etc/apache2/ssl
- sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

No se debe olvidar añadir en el archivo `/etc/apache2/sites-available/default-ssl` las líneas:
- SSLEngine on
- SSLCertificateFile  /etc/apache2/ssl/apache.crt 
- SSLCertificateKeyFile /etc/apache2/ssl/apache.key

El último paso será la configuración del certificado en la máquina balanceadora. Según la recomendación del profesor, la prueba en esta guía se hara en Nginx. 
Para hacer esto se copia el certificado mediante el mismo método seguido en las otras máquinas, añadiendo las líneas necesarias al fichero de configuración /etc/nginx/conf.d/default.conf:
- ssl on;
- ssl_certificate /etc/ngynx/ssl/ngynx.crt;
- ssl_certificate_key /etc/ngynx/ssl/ngynx.key;

Esto debería bastar en la configuración.

## Configuración del cortafuegos

```shell
#!/bin/sh

# Eliminar todas las reglas
iptables -F
iptables -X
iptables -Z
iptables -t nat -F

# Denegar todo el tráfico
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

# Permitir cualquier acceso desde localhost
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

# Abrir los puertos 22, 80 y 433 para permitir el acceso por SSH
iptables -A INPUT -i enp0s8 -p tcp -m multiport --dports 22,80,443 -m --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -o enp0s8 -p tcp -m multiport --sports 22,80,443 --state ESTABLISHED -j ACCEPT

iptables -L -n -v
```

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P3/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P5/README.md)
