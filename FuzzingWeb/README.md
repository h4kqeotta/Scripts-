#  Fuzzing web con BASH


<img width="679" height="383" alt="1" src="https://github.com/user-attachments/assets/c2371383-70c2-4cba-a376-1e12a097dcbe" />


## 🔎 Explicación

En este script se establece un condicional para que si los parametros no son igual a 2 se envie un mensaje de error para que el usuario ponga el diccionario y la url.

Luego se guarda el primer parametro en la variable diccionario y el segundo en url.

Despues se crea una variable “total_lineas” la cual almacena la cantidad de linea que tiene el diccionario proporcionado y se crea otra variable “linea_actual” la cual se utilizará como contador para indicar que linea esta leyendo.

Posteriormente se realiza un bucle while el cual leera el diccionario proporcionado y guardará cada linea en la variable “linea”.

El contador “linea_actual” se incrementará en 1 cada vez que se ejecute el bucle e imprimirá un progreso que indica la linea actual y al lado la cantidad de lineas totales, el \r es necesario para evitar errores en el output.

Luego se crea una variable llamada “respuesta” la cual almacenará el output del comando de sistema de curl, la cuál imprime el codigo de estado de esa url+linea, y en el condiciional if, si el codigo de estado es exitoso se imprimirá un mensaje
