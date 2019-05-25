# Práctica 5. Servidor de disco NFS

En esta práctica se ha configurado el servidor de nginx como servidor NFS y se ha exportado un espacio del disco (un directorio) a los servidores clientes SWAP01 y SWAP02.

## Configurar el servidor NFS

Instalamos las herramientas necesarias:

    sudo apt install nfs-kernel-server nfs-common rpcbind

Posteriormente creamos la carpeta que vamos a compartir y les cambiamos los permisos para que sea accesible por todos:

    mkdir /dat/compartida
    sudo chown nobody:nogroup /dat/compartida/
    sudo chmod -R 777 /dat/compartida/

Añadimos las IP de los clientes al fichero */etc/exports* para que tengan permiso para acceder a los recursos compartidos y reiniciamos el servicio:

    /dat/compartida/ 192.168.56.6(rw) 192.168.56.6(rw)
    sudo service nfs-kernel-server restart

## Configurar los clientes

Al igual que en el servidor instalamos las herramientas necesarias y creamos un directorio en el home del usuario que servirá como punto de montaje:

    sudo apt-get install nfs-common rpcbind
    cd /home/usuario
    mkdir carpetacliente
    chmod -R 777 carpetacliente

> Esta operación se repetirá en ambos clientes

Por último, para que se monte el directorio compartido automaticamente cuando se inicien los sistemas añadimos la siguiente línea al fichero */etc/fstab*:
![configuración fstab](img/fstab.png)