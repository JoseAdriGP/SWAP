# Práctica 3: Balanceo de carga

## Objetivos
En esta práctica configuraremos una red entre varias máquinas de forma que tengamos un balanceador que reparta la carga entre varios servidores finales.
El problema a solucionar es la sobrecarga de los servidores. Se puede balancear cualquier protocolo, pero dado que esta asignatura se centra en las tecnologías web, balancearemos los servidores HTTP que tenemos configurados.
De esta forma conseguiremos una infraestructura redundante y de alta disponibilidad.

## Priemros pasos
Vamos a comprobar que tenemos nuestros dos servidores funcionando y conectados a la misma red, asi que hay que comprobar que Apache está arrancado, obtener la dirección IP de ambos servidores, y ver que sean visibles entre sí.

| Server 1 | Server 2 | 
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

## Instalar NginX
Para instalar NginX, lo primero que necesitamos es importar la clave del repositorio de software:

| Importar clave del repositorio | Añadir repositorio a **“/etc/apt/sources.list”** | Instalación | 
| :-------------: | :-------------: |  :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

La configuración de NginX se realiza en el archivo **“/etc/nginx/conf.d/default.conf”**. La configuración realizada nos permitirá redirigir el tráfico a un grupo de servidores que hemos llamado `apaches` (usando la directiva `upstream`), en dicho grupo de servidores hemos configurado que el primer servidor (`server 192.168.78.132 weight=2`) reciba el doble de carga que el servidor segundo (`server 192.168.78.133 weight=1`), el algoritmo para repartir la carga no es necesario que lo indiquemos porque se toma **“round robin”** por defecto al usar la directiva `upstream`. Dentro de la directiva `location`, deberemos indicar con la directiva `proxy_pass` hacia donde vamos a redirigir el tráfico entrante, en nuestro caso la dirección de acceso a nuestro grupo de servidores ([http://apaches](http://apaches)). Por último, debemos indicar que en los backends la versión de HTTP es 1.1 (`proxy_http_version 1.1`), y que se elimine la cabecera `Connection` (`proxy_set_header Connextion “”`).

| Configuración |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

Una vez completada la configuración, vamos a comprobar que las peticiones al servidor están sindiendo balanceadas según el criterio indicado, para ello mediante el navegador `curl` accederemos al balanceador mediante su dirección IP.

| Comprobar IP | Uso de la orden `curl` para comprobar funcionamiento| 
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

## Instalar HAProxy
Instalamos HAProxy sin ninguna complicación usando `apt-get`. Y procedemos a realizar su configuración en el archivo **“/etc/haproxy/haproxy.cfg”**. Para indicar que vamos a realizar el reparto de carga mediante un algoritmo **“round robin”**, dentro de la directiva `defaults` añadimos la directiva `balance roundrobin`, así conseguiremos que se aplique a todos los servidores. En la directiva `frontend http-in`, indicamos que se debe escuchar el tráfico del puerto 80 (`bind *:80`) y redirigirlo hacia los servidores finales (`default_backend servers`). El grupo de servidores `servers` al que redirigimos el tráfico necesitaremos especificarlo con la directiva `backend servers` añadiéndole las líneas que representan cada uno de los servidores el primer servidor con el doble de carga (`server m1 192.168.78.132:80 maxconn 32 weight 64`) y el segundo servidor con la mitad de carga (`server m2 192.168.78.133:80 maxconn 32 weight 34`). Para establecer la ponderación indicaremos con `weight` un valor proporcional en base a 100 sobre la carga que recibirá dicho servidor.

| Modificación del archivo |
| :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |

## Último paso
Ya simplemente nos falta comprobar la dirección IP de nuestra máquina, para así poder realizar la misma prueba que realizábamos con NginX. Una vez conozcamos la IP, simplemente deberemos lanzar el servicio con el comando `/usr/sbin/haproxy –f /etc/haproxy/haproxy.cfg`, y ya podremos comprobar el funcionamiento con `curl`.

| Comprobar IPs | Comprobar funcionamiento | 
| :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/Images/Comprobaci%C3%B3nIPs.PNG) |


[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P4/README.md)
