# Automatización de Cracking de Contraseñas

<img width="429" height="480" alt="1" src="https://github.com/user-attachments/assets/3eb7e080-4c91-45a9-881f-84ce20a85eb7" />

## 🔎 Explicación


Este script de Bash automatiza el proceso de crackeo de contraseñas por fuerza bruta (usando un diccionario) para dos tipos de archivos protegidos: .zip y bases de datos de contraseñas .kdbx (KeePass) mediante la herramienta John the Ripper.

# 1. Cabecera y validación de argumentos


#### #!/bin/bash

#### if [ $# -ne 2 ]; then
####   echo "Ingresar <DICCIONARIO> <ARCHIVO>"
####     exit 1
#### fi

#!/bin/bash: Indica que el archivo debe ejecutarse usando el intérprete de Bash.

$# -ne 2: Comprueba si el número de argumentos pasados al script no es igual a 2.

Si el usuario no pasó exactamente 2 argumentos, muestra cómo se usa (Ingresar <DICCIONARIO> <ARCHIVO>) y termina la ejecución con código de error exit 1.

2. Asignación de variables


#### diccionario="$1"
#### archivo="$2"

Guarda el primer argumento (la ruta al diccionario/wordlist, por ejemplo rockyou.txt) en la variable diccionario.

Guarda el segundo argumento (el archivo protegido, por ejemplo backup.zip) en la variable archivo.

3. Detección del formato y extracción del hash (case)


#### case "$archivo" in
####     *.zip)
####         zip2john "$archivo" > hash
####         ;;
####     *.kdbx)
####         keepass2john "$archivo" > hash
####        ;;
####     *)
####         echo "Esa opción es incorrecta"
####        exit 1
####        ;;
#### esac

Evalúa la extensión de $archivo:

*.zip: Si termina en .zip, ejecuta zip2john para extraer el hash de la contraseña del archivo ZIP y lo redirige (>) a un archivo temporal llamado hash.

*.kdbx: Si termina en .kdbx, ejecuta keepass2john para extraer el hash de la base de datos de KeePass y lo guarda en hash.

*): Si tiene cualquier otra extensión, muestra "Esa opción es incorrecta" y aborta la ejecución con exit 1.

4. Ataque por diccionario y limpieza


#### john --wordlist="$diccionario" hash
#### john --show hash
#### rm hash

john --wordlist="$diccionario" hash: Ejecuta John the Ripper para intentar descifrar el hash extraído comparándolo con las contraseñas del diccionario proporcionado.

john --show hash: Muestra en la terminal la contraseña en texto plano si John the Ripper logró crackearla.

rm hash: Elimina el archivo temporal hash generado en los pasos anteriores.
