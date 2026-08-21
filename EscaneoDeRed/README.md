#  Escaneo a toda la red con BASH


<img width="1336" height="373" alt="1" src="https://github.com/user-attachments/assets/9bbbe8d5-49de-40b8-b035-fd71ae7f4751" />

## 🔎 Explicación


En este script primero se realiza un filtrado de la interfaz de red y el resultado se lo guarda en la variable “red”. Luego se escanean todas los host disponibles en la red y se hace un filtrado para solo obtener la ips de cada uno y se lo guarda en un archivo “ips.txt”.

Despues hacemos un bucle while para leer linea por linea el contenido del archivo “ips.txt” y  le realizamos un escaneo con nmap a cada linea (ip).

Por ultimo se borra el archivo ips.txt para no dejar basura en el directorio.
