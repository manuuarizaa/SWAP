# Práctica 1

## 1. Instalación de Ubuntu Server
Para estas prácticas voy a usar el programa VirtualBox para virtualizar Ubuntu Server 16.

En VirtualBox creamos una máquina virtual y seleccionamos la imagen ISO previamente descarga de La página oficial de [descargas de Ubuntu](http://releases.ubuntu.com/16.04/ubuntu-16.04.6-server-amd64.iso.torrent?_ga=2.180834471.1612179928.1552479957-1800245174.1551830283)

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/inicioMV.png)

Una vez arrancamos Ubuntu Server, seleccionamos instalarla y ponemos nuestros datos personales, aparecerá un **instalador de programas** en el que se seleccionan, con la tecla *espacio*, **LAMP server** y **OpenSSH server** y a continuación pulsamos *enter* y continuamos con la instalación de Ubuntu Server, todo por defecto.

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/ConfiguracionLAMPySSH.png)

## 2. Configuración de Ubuntu Server
Una vez tengamos Ubuntu Server instalado, configuramos la contraseña del usuario ROOT con el comando: **sudo passwd root**
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/ArranqueInicialMV.png)

En segundo lugar, se apaga la máquina y nos vamos desde VirtualBox a la configuración. En el apartado **RED**, se pulsa en **Adaptador 2** y se selecciona la opción que pone: **Red interna**.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/ConfigRed.png)

Una vez hecho esto, ya podemos encender de nuevo la máquina virtual, pero ahora tenemos que configurar ese nuevo adaptador de red, para ello nos vamos a, usando **vi**, **/etc/network/interfaces**.
Pulsamos **i** y ya podemos insertar la configuración de la segunda red como se muestra a continuación en la imagen:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/ArchivoEtcNetworkInterfaces.png)
Una vez lo tenemos así, pulsamos escape y escribimos **:wq** para guardar los cambios y salir de la edición del archivo.

A continuación, levantamos la red con el comando: **ifup enp0s8**

Ahora pasamos a comprobar si todo funciona correctamente, para ello usaremos el comando **ifconfig** para ver si la red se ha levantado correctamente y haremos un **ping**, por ejemplo, a Google.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/ComprobacionRed.png)
Si nos da problemas con la conexión a internet y hemos comprobado que todo en el archivo configurado anteriormente es correcto, debemos usar el comando: **systemctl restart networking.service** y reiniciamos la máquina virtual.

Una vez tengamos todo funcionando podemos clonar la máquina virtual y configurar el archivo **/etc/network/interfaces** de la nueva MV cambiando, por ejemplo, el address a 192.168.1.200.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/configAmbasMaquinas.png)

## 3. Resolución de actividades propuestas

### Creación del archivo hola.html
Se nos pide crear un archivo en **/var/www/html** llamado **hola.html**, para ello, usando de nuevo **vi** podemos crearlo: **vi /var/www/html/hola.html**
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/holahtml.png)
Esto debemos hacerlo en ambas MV.

### Uso de CURL
Ahora podemos pasar a hacer los **curl** de una máquina a otra para poder visualizar los archivos **hola.html** de la otra máquina.
Para ello ponemos en la terminal **curl http://192.168.1.X/hola.html** , siendo **X** el número que hemos puesto nosotros en la configuración de la otra máquina.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/CurlAmbasMaquinas.png)

### Uso de SSH
Para el SSH usamos **ssh usuario@192.168.1.X** , contestamos *yes* a la pregunta que nos piden, contestamos a los datos que pide y ya estaremos conectados a la otra MV con **ssh**, para comprobar que funciona nos situamos en el mismo directorio, hacemos un **ls** en ambas MV y comprobamos los archivos que hay, desde donde hemos hecho *ssh* creamos un archivo: **touch nuevoArchivo** y hacemos un *ls* en la máquina en la que nos estan haciendo el *ssh* y comprobamos que remotamente hemos creado un nuevo archivo.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica1/SSHAmbasMaquinas.png)













