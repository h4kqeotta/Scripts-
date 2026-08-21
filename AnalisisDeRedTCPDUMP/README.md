#  Análisis de tráfico de red con Tcpdump y Bash


tcpdump sirve para acceder al trafico de red que fluje por una determina interfaz y para luego poder analizarlo con wireshark, el comando sería:

tcpdump -i eth0 icmp (Este comando intercepta todos los paquetes icmp de la interfaz eth0 )

tcmpdump -i eth0 -w trafico.pcap (Este comando intercepta todos los paquetes de la interfaz eth0 y las guarda en un archivo llamado trafico.pcap)

<img width="375" height="304" alt="1" src="https://github.com/user-attachments/assets/ae29f5de-4d5b-4f93-9319-9a90759b72b9" />

## 🔎 Explicación


En este script se utiliza el comando tcpdump para capturar el trafico de la interfaz eth0 y guardarlo en el archivo “captura.pcap” y luego se lo pone en segundo plano para que se ejecute sin problemas por más que lo cierren.

Luego le damos 5 segundos de espera para que se ejecute bien

Y levantamos un servidor python al puerto 80 

Posteriormente, en este caso le agregamos 200 segundos para la recolección de trafico

Después creamos variables con las PID de cada herramienta con el comando pgrep + nombre

Y por ultimo matamos los procesos con el comando kill y el pid recolectado anteriormente
