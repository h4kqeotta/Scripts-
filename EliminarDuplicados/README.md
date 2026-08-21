#  Detección y Eliminación de Archivos Duplicados

fdupes sirve para identificar archivos repetidos

<img width="506" height="325" alt="1" src="https://github.com/user-attachments/assets/0c3862ef-c5bf-4afb-b4ed-8cd67874ae72" />


## 🔎 Explicación

En este script se abre un condicional if para que si NO está instalado el fdupes imprima un mensaje diciendo que lo instale y lo ejecuta en segundo plano y envia todo el output al /dev/null.

Luego se crea una variable “archivos” la cual almacenará los archivos duplicados detectados por fdupes.

Por ultimo de hace un echo de la variable “archivos” y se lee linea por linea con el bucle while almacenando cada linea del archivo en la varible “linea” para luego eliminarlos.
