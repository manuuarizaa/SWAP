# Práctica 2

## Objetivos:
- aprender a copiar archivos mediante ssh
- clonar contenido entre máquinas
- configurar el ssh para acceder a máquinas remotas sin contraseña
- establecer tareas en cron

## Probar el funcionamiento de la copia de archivos por ssh
Encendemos ambas máquinas virtuales y lo primero que hacemos es comprobar que tenemos conexión entre ambas, para ello hacemos en cada una, un ping a la otra máquina.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/ComprobacionPing.png)

Creamos un archivo llamado **nuevoArchivo** en una máquina y, en mi caso, pongo: **tar czf - nuevoArchivo.txt | ssh 192.168.1.200 'cat > ~/tar.tgz'**
Ahora ponemos *yes* y la contraseña de la máquina donde vamos a crear el *tar.tgz* y como podemos comprobar, la copia se ha realizado correctamente por ssh.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/pruebaCopiaSSH.png)

## Clonado de una carpeta entre las dos maquinas
Como detalle previo, tenemos que hacer que el usuario sea el dueño de la carpeta donde residen los archivos que hay en el espacio web, esto se realiza en ambas máquinas.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica2/UsoChown.png)

## Configuración de ssh para acceder sin que solicite contraseña

## establecer una tarea en cron que se ejecute cada hora para mantener actualizado el contenido del directorio /var/www entre las dos máquinas

