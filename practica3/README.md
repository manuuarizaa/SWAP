# Práctica 3

## Objetivos:
En esta práctica se llevarán a cabo, como mínimo, las siguientes tareas:
1. configurar una máquina e instalar el nginx como balanceador de carga
2. configurar una máquina e instalar el haproxy como balanceador de carga
3. someter a la granja web a una alta carga, generada con la herramienta Apache Benchmark, teniendo primero nginx y después haproxy.

En las tareas 1 y 2 debemos hacer peticiones a la dirección IP del balanceador y comprobar que realmente se reparte la carga. Para ello, el index.html en las máquinas finales deben ser diferentes para ver cómo las respuestas que recibimos al hacer varias peticiones son diferentes (eso indicará que el balanceador deriva tráfico a las máquinas servidoras finales). Además, se comprobará el funcionamiento de los algoritmos de balanceo round-robin y con ponderación (en este caso supondremos que la máquina 1 tiene el doble de capacidad que la máquina 2).
En la tarea 3 debemos usar la herramienta ab para someter a una carga muy alta (gran número de peticiones y con alta concurrencia) a la granja web, primero estando nginx como balanceador, y a continuación estando haproxy como balanceador. Como resultado, se debe realizar una comparación de los tiempos medios de servicio entre ambos balanceadores, para poder determinar cuál funciona mejor.

## Instalar NGINX como balanceador de carga y configuración
Para instalar NGINX y activarlo correctamente debemos de borrar o detener en la nueva máquina, si es que lo tenemos, el Apache.
Para ello usamos el comando **systemctl stop apache2.service**
y lo activamos con **sudo systemctl start nginx**

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica3/DetenerApacheYactivarNginX.png)

Hacemos una configuración del NGINX en el archivo **/etc/nginx/conf.d/default.conf** de la siguiente manera:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica3/configuracionNGINX.png)


Ademas le añadimos la especificación de que use conexciones con keepalive para que el balanceo sea equilibrado:

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica3/NGINXkeepalive.png)

Comprobamos el estado de NGINX una vez que hemos hecho toda la configuración:

![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica3/statusNginx.png)

## Instalar haproxy como balanceador de carga y configuración
Para instalar haproxy usamos todo tal cual dice el guión de prácticas, para instalarlo usamos el comando **sudo apt-get install haproxy** y luego nos metemos en **/etc/haproxy/haproxy.cfg** y ponemos la siguiente configuración:

**global**
	daemon
	maxconn 256

**defaults**
	mode http
contimeout 4000
clitimeout 42000
srvtimeout 43000

**frontend http-in**
	bind * :80
	default_backend servers

**backend servers**
server m1 192.168.1.100:80 maxconn 32
server m2 192.168.1.200:80 maxconn 32

Una vez todo configurado con haproxy, lo lanzamos con el comando:
**sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg**

En esta máquina vemos que funciona correctamente:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica3/HaproxyFuncionando.png)

Ya podemos, con otra máquina, lanzar peticiones al balanceador, en mi caso es la máquina que termina en **50**.

Vemos el funcionamiento de lo que se nos pide:
![imagen](https://github.com/manuuarizaa/SWAP/blob/master/practica3/p3funcionandohaproxy.png)







