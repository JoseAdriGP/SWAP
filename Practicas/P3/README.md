# Práctica 3: Balanceo de carga

> ## Debido a un problema con el PC en el que estoy trabajando, las capturas de pantalla serán subidas más adelante, por lo que voy a hacer anotaciones con las ordenes que deben usarse


## Objetivos
En esta práctica configuraremos una red entre varias máquinas de forma que tengamos un balanceador que reparta la carga entre varios servidores finales.
El problema a solucionar es la sobrecarga de los servidores. Se puede balancear cualquier protocolo, pero dado que esta asignatura se centra en las tecnologías web, balancearemos los servidores HTTP que tenemos configurados.
De esta forma conseguiremos una infraestructura redundante y de alta disponibilidad.

## Primeros pasos
Vamos a comprobar que tenemos nuestros dos servidores funcionando y conectados a la misma red, asi que hay que comprobar que Apache está arrancado, obtener la dirección IP de ambos servidores, y ver que sean visibles entre sí.

| Server 1 | Server 2 | 
| :-------------: | :-------------: |
| ![Imagen]() | ![Imagen]() |

#### Anotaciones 
- Comprobar el funcionamiento de apache: /etc/init.d/apache2 status
- Datos del servidor: ifconfig

## Instalar NginX
> Para instalar NginX se necesita importar la clave del repositorio de software.

| Importar clave del repositorio | Añadir repositorio a **“/etc/apt/sources.list”** | Instalación | 
| :-------------: | :-------------: |  :-------------: |
| ![Imagen]() | ![Imagen]() | ![Imagen]() |

#### Anotaciones 
- Obtener repositorio: wget http://nginx.org/key/ngonx_signing.key
- Añadir clave: apt-key add /tmp/nginx_signing.key
- Quitar directorio: rm -f /tmp/nginx_signing.key
- Añadir repositorio a /etc/aptsources.list: deb http://nginx.org/packages/ubuntu/ lucid nginx y deb-src http://nginx.org/packages/ubuntu/ lucid nginx
- Instalación: sudo apt-gt install nginx

La configuración de NginX se realiza en el archivo **“/etc/nginx/conf.d/default.conf”**. La configuración realizada permite redirigir el tráfico a un grupo de servidores que en este caso se llama `apaches` (usando la directiva `upstream`). En dicho grupo de servidores se configura el primer servidor (`server 192.168.78.132 weight=2`) para que reciba el doble de carga que el segundo (`server 192.168.78.133 weight=1`). No es necesario determinar el algoritmo para repartir la carga, pues se toma **“round robin”** por defecto al usar la directiva `upstream`. Dentro de la directiva `location`, se debe indicar con la directiva `proxy_pass` hacia donde se va a redirigir el tráfico entrante. En el caso actual, la dirección de acceso al grupo de servidores ([http://apaches](http://apaches)). Por último, se debe indicar que en los backends la versión de HTTP es 1.1 (`proxy_http_version 1.1`), y que se elimine la cabecera `Connection` (`proxy_set_header Connextion “”`).

| Configuración |
| :-------------: |
| ![Imagen]() |

Una vez completada la configuración, se comprobará que las peticiones al servidor están sindiendo balanceadas según el criterio indicado. Mediante el navegador `curl` se accede al balanceador mediante su dirección IP.

| Comprobar IP | Uso de la orden `curl` para comprobar funcionamiento| 
| :-------------: | :-------------: |
| ![Imagen]() | ![Imagen]() |

#### Anotaciones 
- El uso de curl en mi caso es: curl http://192.168.78.134

## Instalar y configurar HAProxy
Para instalar HAProxy se usa `apt-get`. 
Para realizar su configuración habrá que modificar en el archivo **“/etc/haproxy/haproxy.cfg”**. Para indicar que se va a realizar el reparto de carga mediante un algoritmo **“round robin”**, dentro de la directiva `defaults` se añade la directiva `balance roundrobin`, consiguiento así que se aplique a todos los servidores. En la directiva `frontend http-in`, se indicará que debe escuchar el tráfico del puerto 80 (`bind *:80`) y redirigirlo hacia los servidores finales (`default_backend servers`). El grupo de servidores `servers` al que se redirige el tráfico hay que especificarlo con la directiva `backend servers` añadiéndole las líneas que representan cada uno de los servidores el primer servidor con el doble de carga (`server m1 192.168.78.132:80 maxconn 32 weight 64`) y el segundo servidor con la mitad de carga (`server m2 192.168.78.133:80 maxconn 32 weight 34`). Para establecer la ponderación se indica con `weight` un valor proporcional en base a 100 sobre la carga que recibirá dicho servidor.

| Archivo de configuración |
| :-------------: |
| ![Imagen]() |

## Último paso
Ahora se pasa a comprobar la dirección IP de la máquina para así poder realizar la misma prueba que se realizaba con NginX. Una vez conocida la IP, se deberá lanzar el servicio con el comando `/usr/sbin/haproxy –f /etc/haproxy/haproxy.cfg`, de forma que ya se podrá comprobar el funcionamiento con `curl`.

| Comprobar IPs | Comprobar funcionamiento | 
| :-------------: | :-------------: |
| ![Imagen]() | ![Imagen]() |


[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P2/README.md) [Siguiente](https://github.com/JoseAdriGP/SWAP/blob/master/Practicas/P4/README.md)
