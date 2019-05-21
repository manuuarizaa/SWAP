# Práctica 5
## Tareas a realizar:
1. Crear una BD con al menos una tabla y algunos datos.
2. Realizar la copia de seguridad de la BD completa usando mysqldump en la máquina principal y copiar el archivo de copia de seguridad a la máquina secundaria.
3. Restaurar dicha copia de seguridad en la segunda máquina (clonado manual de la BD), de forma que en ambas máquinas esté esa BD de forma idéntica.
4. Realizar la configuración maestro-esclavo de los servidores MySQL para que la replicación de datos se realice automáticamente.

## Crear BD e introducir algunos datos
Entramos a Mysql y creamos una base de datos, posteriormente, creamos la tabla e introducimos algunos datos.

Creacion de BD y tabla:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/1-CreacionBaseDeDatosMYSQL.png)

Introduccion de datos:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/2-CrearBDeIntroducirDatos.png)


## Copia de seguridad con MYSQLDUMP
Antes de hacer la copia evitamos el acceso a la BD:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/3-AntesDeCopiaEvitarAccederBD.png)

Y ahora ya si podemos hacer el mysqldump para guardar los datos:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/4-mysqldumpAtablaContactosYDesbloquearTablas.png)

Una vez tenemos la copia de seguridad hecha, pasamos los archivo a la máquina secundaria (máquina 2):
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/5-CopiarArchivoAM2.png)

## Restaurar copia de seguridad en la segunda máquina
Para esto, creamos una base de datos con mysql en la máquina secundaria y procedemos a volcar los datos de nuestra copia de seguridad en ella:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/6-CrearBDyVolcarDatos.png)

## Realizar la configuración maestro-esclavo
Lo primero que tenemos que hacer es la configuración de MYSQL del maestro, para ello editamos como superusuario el archivo **/etc/mysql/mysql.conf.d/mysqld.cnf** 

Comentamos bind-address 127.0.0.1, indicamos el archivo donde almacenar el log de errores, establecemos el id del servicio (1 para el maestro), activamos el registro binario y por último guardamos  el documento y reiniciamos el servicio:

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/7-ConfiguracionArchivo1.png)

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/7-ConfiguracionArchivo2.png)

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/8-RestartEMaestroYCorrecto.png)

Hacemos lo mismo para el esclavo pero poniendo como identificador de servicio el número 2. Reiniciamos el servicio y comprobamos que todo es correcto:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/9-RestartEsclavoYCorrecto.png)

Volvemos al maestro y creamos un usuario y le damos permiso de acceso para la replicación, además obtenemos los datos de la BD que vamos a replicar para posteriormente usarlos en la configuración del esclavo:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/11-UsuarioYPermisoReplicacion.png)

Volvemos a la máquina esclava (máquina 2) y le damos los datos del maestro (La ip del maestro es la terminada en .100):
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/12-PermisosYStartDemonio.png)

Por último arrancamos el esclavo y ya esta todo listo para que los demonios de MySQL de las dos máquinas repliquen automáticamente los datos que se introduzcan/modifiqeun/borren en el servidor maestro.

Volvemos al maestro y desbloqueamos las tablas con **UNLOCK TABLES** y por último, si queremos asegurarnos que de todo es correcto, nos vamos al esclavo, ejecutamos el comando **SHOW SLAVE STATUS \G** y la variable **Seconds_Behind_Master** debe ser 0, si es null es que hay algún fallo.
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/13-SecondsBehinMasterDistintoNull.png)

Ahora ya podemos comprobar el correcto funcionamiento:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica5/14-FuncionamientoCorrectoM-S.png?raw=true)





