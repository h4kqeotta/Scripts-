#  Host Discovery

<img width="551" height="336" alt="1" src="https://github.com/user-attachments/assets/d625af63-2462-4007-8ee7-d86ac2ba6ec5" />

## 🔎 Explicación


En este script lo que se hace es establecer una funcion con control_c para que cuando el usuario lo aprete se salga del programa y envie un mensaje. Luego se hace un bucle for para recorrer todas las ips de 1 a 255 con un tiempo de espera de 0.5 ms y se le agrega el bash -c para poder agregar un comando entre comillas.

Despues de eso se crea un condicional if para que si el comando anterior es igual a 0, osea exitoso, se imprima un mensaje de actividad, y al final se agrega un progreso como decoración.
