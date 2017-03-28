# Práctica 1: Preparación de herramientas

> La maquina qu virtual que voy a utilizar para esta práctica es VMware Player 12, por lo que al tener una instalación automática, tendremos que hacer una serie de pasos extra para tener completamente configurada la herramienta.

## Instalación de Ubuntu Server 16.04.2

| Presionamos en "Create a New Virtual Machine" | Indicar el archivo ISO de la distribución | Rellenar datos de usuario |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-1.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-2.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-3.PNG)

| Rellenar datos para la máquina virtual | Indicar el espacio que queremos para la máquina y el método de almacenamiento | Finalizar la configuración inicial |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-4.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-5.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-6.PNG)

> Si es la primera vez que utilizas VMware player en tu equipo, es probable que te pida que instales una serie de herramientas para el sistema operativo que este instalando. De ser el caso, recomiendo aceptar esta instalación, ya que no afecta a la otra instalación, simplemente al terminar solicitará reiniciar el sistema donde tengamos instalado VMware. 

## Instalación de ubuntu

Como ya hemos comentado anteriormente, la instalación que realiza es automática sin posibilidad de customizar. Esto provoca que el teclado se instale en inglés 

| Proceso de instalación | Iniciar sesión | Logueo correcto y orden para cambiar configuración de teclado |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-7.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-8.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-9.PNG)


## Configuración de teclado

| Selección de "Generic 104-key PC" | Selección Spanish | Selección de uso de Winkeys |
| :-------------: | :-------------: | :-------------: |
| ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-11.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-12.PNG) | ![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-13.PNG)

Las opciones anteriores son realmente las primeras, luego pide más configuraciones. Teniendo en cuenta que esto variará en función del teclado que tengamos, me ha parecido un error ponerlas. 

## Instalación de apache

Como podemos ver, la orden "sudo apt-get install apache2 mysql-server php5 libapache2-mod-php5 php5-mysql" no pudo ejecutarse por problemas al faltar repositorios, pero el fallo persiste después de utilizar "sudo apt-get update" y "sudo apt-get update". 

![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-14.PNG)
> La solución fue instalar utilizando la siguiente orden. 
![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-15.PNG)

 Como podemos ver en la siguiente captura de pantalla, la versión que utilizo es "Apache/2.4.18 (Ubuntu)" y apache está funcionando correctamente.
 
![Imagen](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/Practicas/P1/Images/p1-16.PNG)

## Instalación de ssh

Puesto que no que la instalación no se ha producido de forma automática, habrá que asegurarsse de la instalación de ssh. Para ello, la orden que debemos realizar es "sudo apt-get install openssh-server".

Cuando se ha hecho la instalación, estas son algunas de las ordenes que pueden resultar útiles:
- **Editar la configuración del servidor SSH:** "sudo gedit /etc/ssh/sshd_config"
- **Arrancar el servidor:** "sudo /etc/init.d/ssh start"
- **Parar el servidor:** "sudo /etc/init.d/ssh stop"
- **Reiniciar el servidor:** "sudo /etc/init.d/ssh restart"


[Volver](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md)
