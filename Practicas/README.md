# Problemas encontrados

## apt no nos permite instalar nada

Me ocurrió que al instalar la segunda máquina virtual (en VMware Player no puedes clonar), aparecía lo siguiente cuando quería instalar apache y ssh:

    media change: please insert the disc labeled
    'Ubuntu 12.04.3 LTS _Precise Pangolin_ - Release amd64 (20130820.1)
    in the drive '/media/cdrom/' and press enter

Para solucionar esto, debemos quitar cdrom del archivo source.list mediante:
    
    sudo sed -i '/cdrom/d' /etc/apt/sources.list

Cuando hayamos hecho esto, utilizaremos "sudo apt-get update" y "sudo apt-get upgrade" y podremos trabajar con normalidad. 

## No existe el archivo authorized_keys

Puede ocurrir que el archivo authorized_keys no este creado, por lo que habrá que crearlo de forma manual:
cd ~/.ssh
touch authorized_keys
chmod 600 authorized_keys
cat id_dsa.pub >> authorized_keys
rm id_dsa.pub

Es importante dar los permisos adecuados despúes de crear el archivo correspondiente.

[Indice](https://github.com/JoseAdriGP/SWAP-Practicas/blob/master/README.md) [Anterior](https://github.com/JoseAdriGP/SWAP/blob/master/Ejercicios/T4.md) 

