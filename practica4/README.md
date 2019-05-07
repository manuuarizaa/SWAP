# Práctica 4. Asegurar la granja web

## Instalar el certificado SSL
Con root ejecutamos:
**a2emod ssl
service apache 2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
/etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt**

e introducimos los datos que nos pide:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/CrearSSL.png)

Ahora vamos al archivo de configuración del sitio default-ssl y añadimos lo siguiente tras la linea donde pone **SSLEngine**:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/LineasDebajoEngineSSLon.png)

Activamos el sitio default-ssl y reiniciamos apache con:
a2ensite default-ssl
service apache2 reload

Ahora con **CURL** comprobamos que se puede acceder a la máquina 1 mediante**https**:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/curlhttpsMaquina1.png)

## Balanceo con https
Para la máquina 2, copiamos los certificados y activamos el sitio y reiniciamos tal como hicimos en la máquina 1, el método que se ha usado para copiar los certificados de una máquina a otra es el siguiente:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/MetodoCopiarCRTyKey.png)

y así con todas las máquinas que tengamos en la granja web.

Una vez hemos hecho esto, en el balanceador nginx debemos añadir al archivo **/etc/nginx/conf.d/default.conf** lo siguiente:
**listen 443 ssl;
ssl on;
ssl_certificate /certificados/apache.crt;
ssl_certificate_key /certificados/apache.key;**

(en mi caso la carpeta es "certificados", depende de donde las alojemos en el balanceador debemos cambiar esta parte del código)
y reiniciarlo.

Ahora ya podemos hacer peticiones con https al balanceador y comprobamos que lo hace correctamente:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/FuncionaCurlHTTPS.png)

## Configurar cortafuegos
Para configurar el cortafuegos creamos un script, en mi caso en la carpeta scripts, como sigue:

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/scriptIPTABLESfuncionando.png)

Y como vemos al ejecutarlo funciona correctamente.

Una vez tenemos esto debemos hacer que se ejecute al inicio de la máquina, para ello se puede hacer de dos maneras, con crontab o ,como mejor me ha funcionado, editando el archivo **/etc/profile**:

**Método 1: CRONTAB**
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/opcion1deInicioCrontab.png)

**Método 2: /etc/profile**
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica4/EjecutarScriptInicio.png)
Con esta opción, al iniciar nos saldrá un mensaje por pantalla que dirá **SCRIPT FUNCIONANDO**, señal que se puso para comprobar que se había ejecutado correctamente.