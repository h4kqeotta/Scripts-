# Detector de Sistemas Operativos

<img width="505" height="266" alt="1" src="https://github.com/user-attachments/assets/181bb914-f414-4013-8e0f-b6af5701af3b" />

## 🔎 Explicación

En este script se almacena en la variable host la ip de la maquina. Luego se escribe un mensaje y se realiza un ping al host y se almacena el resultado en el fichero ping.log.

Luego se abre un condicional if en el cual test ejecutará un comando a nivel de sistemas que si existe la string ttl=64 en el fichero ping.log emitirá un numero, 0 o 1, dependiendo si existe o no, y si existe (1) se imprime un mensaje. Lo mismo en el elif de abajo pero con 128 que es el ttl de Windows.


<img width="557" height="356" alt="1" src="https://github.com/user-attachments/assets/44cdd053-1ff4-4fc9-b337-178e5e076e4c" />


el comando sin el “-c” busca la strings en el fichero y lo marcha, en cambio

grep ttl=64 ping.log  -c 1 expresa su existencia en numeros, en la cual si existe nos muestra un 1 y si no existe nos muestra un 0.

Tambien podemos ejecutar el mismo comando pero con una sintaxys basada en bash con los [ ] de la siguiente forma:

<img width="470" height="253" alt="1" src="https://github.com/user-attachments/assets/44729eee-9654-4f94-b4b9-51fcd3ad88cc" />
