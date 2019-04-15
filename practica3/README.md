# Práctica 3

## Objetivos:
En esta práctica se llevarán a cabo, como mínimo, las siguientes tareas:
1. configurar una máquina e instalar el nginx como balanceador de carga
2. configurar una máquina e instalar el haproxy como balanceador de carga
3. someter a la granja web a una alta carga, generada con la herramienta Apache Benchmark, teniendo primero nginx y después haproxy.

En las tareas 1 y 2 debemos hacer peticiones a la dirección IP del balanceador y comprobar que realmente se reparte la carga. Para ello, el index.html en las máquinas finales deben ser diferentes para ver cómo las respuestas que recibimos al hacer varias peticiones son diferentes (eso indicará que el balanceador deriva tráfico a las máquinas servidoras finales). Además, se comprobará el funcionamiento de los algoritmos de balanceo round-robin y con ponderación (en este caso supondremos que la máquina 1 tiene el doble de capacidad que la máquina 2).
En la tarea 3 debemos usar la herramienta ab para someter a una carga muy alta (gran número de peticiones y con alta concurrencia) a la granja web, primero estando nginx como balanceador, y a continuación estando haproxy como balanceador. Como resultado, se debe realizar una comparación de los tiempos medios de servicio entre ambos balanceadores, para poder determinar cuál funciona mejor.

## Instalar NGINX como balanceador de carga
Para instalar NGINX y activarlo correctamente debemos de borrar o detener en la nueva máquina, si es que lo tenemos, el Apache.
Para ello usamos el comando **systemctl stop apache2.service**
y lo activamos con **sudo systemctl start nginx**

![imagen]()

Comprobamos el estado del NGINX una vez activado:

![imagen]()


