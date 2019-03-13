# Práctica 2

## Objetivos:
- aprender a copiar archivos mediante ssh
- clonar contenido entre máquinas
- configurar el ssh para acceder a máquinas remotas sin contraseña
- establecer tareas en cron

## Probar el funcionamiento de la copia de archivos por ssh
Encendemos ambas máquinas virtuales y lo primero que hacemos es comprobar que tenemos conexión entre ambas, para ello hacemos en cada una, un ping a la otra máquina.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/ComprobacionPing.png)

Creamos un archivo llamado **nuevoArchivo** en una máquina y, en mi caso, pongo: **tar czf - nuevoArchivo.txt | ssh 192.168.1.200 ' cat > ~/tar.tgz'**
Ahora ponemos *yes* y la contraseña de la máquina donde vamos a crear el *tar.tgz* y como podemos comprobar, la copia se ha realizado correctamente por ssh.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/pruebaCopiaSSH.png)

## Clonado de una carpeta entre las dos maquinas
Como detalle previo, tenemos que hacer que el usuario sea el dueño de la carpeta donde residen los archivos que hay en el espacio web, esto se realiza en ambas máquinas.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/UsoChown.png)
Y hacemos el comando: **rsync -avz -e ssh manuariza@192.168.1.100:/var/www/ /var/www/** en la máquina secundaria para clonar el contenido del servidor web principal en la máquina 2.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/UsoDeClone.png)
Como se puede ver en la imagen, en la maquina principal(la izquierda) creamos un archivo, en la secundaria hacemos el comando y se puede ver como el contenido se ha clonado correctamente.

## Configuración de ssh para acceder sin que solicite contraseña
En la máquina secundaria generamos la clave con: **ssh-keygen -b 4096 -t rsa** 
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/sshSinPass.png)
Ahora hacemos una copia de la clave a la máquina principal (a la que queremos acceder luego desde la secundaria) para que ya no nos pida la contraseña. Para hacer la copia usamos, en mi caso, el comando **ssh-copy-id manuuarizaa@192.168.1.100**
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/SSHNoPideYaPass.png)
De esta manera, cuando hagamos **SSH** a la máquina principal, no nos pedirá la contraseña.

## establecer una tarea en cron que se ejecute cada hora para mantener actualizado el contenido del directorio /var/www entre las dos máquinas
Editamos el archivo **/etc/crontab** y añadimos una tarea nueva, en nuestro caso, clonar cada minuto (en vez de una hora para hacer una comprobación rapida de que funciona) el contenido de la carpeta **/var/www**:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/automatizarTarea.png)
Y ahora comprobamos que la tarea funciona:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/ComprobacionTareaCron.png)
Como vemos,inicialmente no aparece el archivo **miprueba.txt** en la máquina 2, al minuto y tras hacer un *ls* vemos como la tarea ha funcionado correctamente y ya aparece el nuevo archivo.
