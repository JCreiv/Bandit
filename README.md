

**Nivel 1 > 2**
--------------------
### Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

![](./ANEXOS/Pasted%20image%2020241017024452.png)

En el primer nivel, simplemente tenemos que listar un archivo con el nombre `-`. En este caso, lo podemos hacer haciendo un `cat` de la ruta absoluta o utilizando este comando: `cat $(pwd)/-`


---

**Nivel 2 > 3**
--------------------
### Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

![[Pasted image 20241017024606.png]]

En el segundo nivel simplemente usando el tabulador nos autocompletara el nombre con espacios.



---

**Nivel 3 > 4**
--------------------
### Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

![[Pasted image 20241017024758.png]]

En este caso, la contraseña estaba en un archivo oculto y, simplemente con usar `ls -la` o `ll`, veríamos el archivo y podríamos mostrarlo en la terminal.


---

**Nivel 4 > 5**
--------------------
### Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

![[Pasted image 20241017113523.png]]

En este caso, vemos que hay muchos archivos, pero solo uno es legible para humanos. Para encontrar el archivo correcto, usamos `find . | xargs file`.



---

**Nivel 5 > 6**
--------------------
### Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

![[Pasted image 20241017113849.png]]

En este caso, la contraseña para el siguiente nivel está almacenada en un archivo ubicado en algún lugar dentro del directorio **inhere** y tiene todas las siguientes propiedades:

- Es legible para humanos
- Tiene un tamaño de 1033 bytes
- No es ejecutable

Para encontrarlo, usamos el siguiente comando `find`:

```bash
find . -type f ! -executable -size 1033c | xargs cat | xargs
```

**`find . -type f ! -executable -size 1033c`**:

- `find .` busca dentro del directorio actual y sus subdirectorios.
- `-type f` selecciona solo archivos (no directorios).
- `! -executable` excluye los archivos que son ejecutables.
- `-size 1033c` filtra los archivos que tienen un tamaño exacto de 1033 bytes.
- Esto devuelve la ruta de cualquier archivo en el directorio `inhere` que cumpla con esas propiedades.



---

**Nivel 6 > 7**
--------------------
### Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

![[Pasted image 20241017114204.png]]

La contraseña para el siguiente nivel se almacena en algún lugar del servidor y tiene todas las propiedades siguientes: 

- propiedad del usuario bandit7 
- propiedad del grupo bandit6 
- 33 bytes de tamaño

Para ello usamos el comando: 

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
```

- **`find / -type f -user bandit7 -group bandit6 -size 33c`**:
    
    - `find /` busca en el sistema de archivos desde la raíz (`/`).
    - `-type f` filtra solo archivos.
    - `-user bandit7` encuentra archivos que pertenecen al usuario `bandit7`.
    - `-group bandit6` selecciona archivos que pertenecen al grupo `bandit6`.
    - `-size 33c` busca archivos que tengan exactamente 33 bytes de tamaño.
    - Este comando devuelve las rutas de los archivos que coinciden con estas características.
- **`2>/dev/null`**:
    
    - Redirige los errores a `/dev/null` para que no se muestren en pantalla. Esto evita ver mensajes de error sobre permisos insuficientes en ciertos directorios.

---

**Nivel 7 > 8**
--------------------
### Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

![[Pasted image 20241017114718.png]]

La contraseña para el siguiente nivel se almacena en el archivo data.txt junto a la palabra `millioth`

Para ello usamos `grep` para buscar esa cadena en el archivo data.txt

---

**Nivel 8 > 9**
--------------------
### Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

![[Pasted image 20241017115111.png]]

La contraseña está en el archivo `data.txt`, que contiene varias líneas de texto. Debes encontrar una línea que contenga solo un valor único.

Usamos `sort` y `uniq` para encontrar una línea única en `data.txt`

```bash 
cat data.txt | sort | uniq -u
```

- **`cat data.txt`**: muestra el contenido del archivo `data.txt`.
    
- **`| sort`**: ordena las líneas alfabéticamente. Esto es importante para que las líneas duplicadas queden juntas, lo cual facilita identificarlas en el siguiente paso.
    
- **`| uniq -u`**: muestra solo las líneas que aparecen una vez en el archivo ordenado, es decir, elimina todas las líneas duplicadas.

---

**Nivel 9 > 10**
--------------------
### Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

![[Pasted image 20241017115323.png]]

En este nivel se hace exactamente igual que el anterior usando el comando `grep` pero buscando una cadena con el símbolo  ` = `


---

**Nivel 10 > 11**
-----------------
### Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

![[Pasted image 20241017115458.png]]

La contraseña para el siguiente nivel se almacena en el archivo data.txt, que contiene datos codificados en base64 para ello usamos el comando `base64` y el parámetro `-d` para decodificar el archivo

---

**Nivel 11 > 12**
----------------
### Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

![[Pasted image 20241017131210.png]]


La contraseña para el siguiente nivel se almacena en el archivo data.txt, donde todas las letras minúsculas (a-z) y mayúsculas (A-Z) se han rotado 13 posiciones, en este caso he usado la pagina web https://rot13.com/ para averiguar la contraseña

---

**Nivel 12 > 13**
--------------------
### Password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

##### decompressor.sh:

```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n[!] Saliendo...\n"
  exit 1
}

# Ctrl+C
trap ctrl_c INT

first_file_name="$1"
decompressed_file_name="$(7z l $1 | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"

7z x $first_file_name &>/dev/null

while [ $decompressed_file_name ]; do
  echo -e "\n[+] Nuevo archivo descomprimido: $decompressed_file_name"
  7z x $decompressed_file_name &>/dev/null
  decompressed_file_name="$(7z l $decompressed_file_name 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
done 
```

![[Pasted image 20241017171916.png]]

La contraseña para el siguiente nivel se almacena en el archivo data.txt, que es un hexdump de un archivo que ha sido comprimido repetidamente. Para este nivel habría que primero revertir el volcado de hexdump con `xxd` y el parámetro `-r` en un archivo `.bin`.

Una vez tenemos el archivo tendremos que descomprimirlo, en este caso lo haríamos con `7z` gracias a la versatilidad de descompresiones que tiene, y el parámetro `x` para descompresión del archivo.

He creado un `.sh` para automatizar la descompresión de un archivo varias veces comprimido.


---

**Nivel 13 > 14**
---------------------
### Password: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

![[Pasted image 20241017183545.png]]

La contraseña para el siguiente nivel se almacena en /etc/bandit_pass/bandit14 y sólo puede ser leída por el usuario bandit14. Como vemos nos dan una clave privada llamada sshkey.private con la cual con `ssh` y el parámetro `-i` podemos pasarle clave privada y nos conectaríamos con bandit14


---

**Nivel 14 > 15**
--------------------
### Password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

![[Pasted image 20241018143306.png]]

La contraseña para el siguiente nivel se puede recuperar enviando la contraseña del nivel actual al puerto 30000 en localhost, para ello usando `nc` y apuntando al puerto que espera la respuesta que en este caso seria la maquina localhost y por el puerto 30000, añadiendo la contraseña de bandit14 nos devolvería la nueva contraseña

---

**Nivel 15 > 16**
--------------------
### Password: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

ncat --ssl localhost 30001

![[Pasted image 20241018144933.png]]

La contraseña para el siguiente nivel se puede recuperar enviando la contraseña del nivel actual al puerto 30001 en localhost utilizando encriptación SSL/TLS.

Para obtener esta contraseña en vez de usar `nc` usaremos `ncat` debido a que `ncat` tiene la opción --ssl para permitir establecer conexiones seguras cifradas utilizando SSL/TLS.

---

**Nivel 16 > 17**
--------------------
### Password: EReVavePLFHtFlFsjn3hyzMlvSuSAcRD

ssh bandit16@bandit.labs.overthewire.org -p2220 

```bash
nmap --open -p31000-32000 localhost | awk -F "/" '/tcp/ {print $1}' | awk 'NR > 1' > ports

for port in $(< ports); do
    echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | timeout 2 bash -c "ncat --ssl localhost $port"
done
```
![[Pasted image 20241021185131.png]]

Las credenciales para el siguiente nivel se pueden recuperar enviando la contraseña del nivel actual a un puerto en localhost en el rango 31000 a 32000. Primero averigua cuáles de estos puertos tienen un servidor escuchando en ellos. Luego averigua cuáles de ellos hablan SSL/TLS y cuáles no. Sólo hay 1 servidor que dará las siguientes credenciales, los demás simplemente te devolverán lo que le envíes.

Sabiendo esta premisa he creado un script que escanea los puertos de un rango específico en `localhost`, guarda los puertos abiertos en un archivo y luego intenta conectarse a cada uno de esos puertos utilizando `ncat` con un mensaje específico, cerrando cada conexión después de 2 segundos. 

Una vez ejecutas ese script en un directorio temporal creado con `mktemp -d` nos devolveran una `clave RSA` privada con la cual podremos conectarnos a bandit 17 usando ssh y ver la contraseña.



---

**Nivel 17 > 18**
--------------------
### Password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO


![[Pasted image 20241021190218.png]]

Hay 2 archivos en el directorio principal: `passwords.old` y `passwords.new`. La contraseña para el siguiente nivel está en passwords.new y es la única línea que ha cambiado entre passwords.old y passwords.new.

Con el comando diff podemos comparar archivos y mostrar las diferencias entre ellos. En este caso nos mostrara la contraseña.

---

**Nivel 18 > 19**
--------------------
### Password: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

![[Pasted image 20241021190850.png]]

La contraseña para el siguiente nivel se almacena en un archivo llamado `readme` en el directorio home. Desafortunadamente, alguien ha modificado el archivo `.bashrc` para cerrar la sesión automáticamente al conectarse por SSH.

Para solucionar este problema, podemos forzar la asignación de un pseudo-terminal (PTY) en la sesión SSH utilizando el parámetro `-t` y ejecutar un comando que muestre el contenido del archivo `readme`.

---


**Nivel 19 > 20**
--------------------
### Password: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

![[Pasted image 20241021191739.png]]

Para acceder al siguiente nivel, debes utilizar el binario **setuid** en el directorio de inicio. Ejecútalo sin argumentos para saber cómo usarlo. La contraseña para este nivel se puede encontrar en el lugar habitual (/etc/bandit_pass), después de haber utilizado el binario **setuid**.

En este caso, se nos proporciona un binario que nos permite ejecutar comandos como si fuéramos `bandit20` , lo que nos ayudará a descubrir la contraseña

---

**Nivel 20 > 21**
--------------------
### Password: EeoULMCra2q0dSkYj561DX7s1CpBuOBt

![[Pasted image 20241021192426.png]]

Hay un binario **setuid** en el directorio home que realiza las siguientes funciones: establece una conexión con `localhost` en el puerto que especifiques como argumento en la línea de comandos. Luego, lee una línea de texto de la conexión y la compara con la contraseña del nivel anterior (bandit20). Si la contraseña es correcta, transmitirá la contraseña del siguiente nivel (bandit21).

```bash
echo "OqXahG8ZjOVWOGhs7tOWsCfZyXOUbYO" | netcat -lp 1234 &

./suconnect 1234
```

Al ejecutar el primer comando, enviamos la contraseña del nivel anterior al puerto **1234** utilizando `echo`. Este puerto permanecerá escuchando a la espera de que se ejecute el binario **setuid** en dicho puerto. El binario leerá la contraseña del nivel anterior (bandit20) y, si es correcta, devolverá la contraseña del siguiente nivel (bandit21).

---

**Nivel 21 > 22**
--------------------
### Password: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

![[Pasted image 20241021192932.png]]

Un programa se está ejecutando automáticamente a intervalos regulares a través de **cron**, el programador de trabajos basado en el tiempo. Debemos buscar en `/etc/cron.d/` la configuración y observar qué comando se está ejecutando.

En este caso, veremos que esta tarea de cron está enviando la contraseña de **bandit22** a un archivo temporal. Para obtener la contraseña, simplemente necesitamos mostrar el contenido de ese archivo.

---

**Nivel 22 > 23**
--------------------
### Password: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

![[Pasted image 20241021193403.png]]

Un programa se está ejecutando automáticamente a intervalos regulares a través de **cron**, el programador de trabajos basado en el tiempo. Debemos buscar en `/etc/cron.d/` la configuración y observar qué comando se está ejecutando.

En este caso, el script que se ejecuta utiliza la variable `myname`, que toma el valor del comando `whoami`. Esto significa que `myname` se asigna a `bandit23`, el usuario actual. Para simplificar el proceso, podemos sustituir manualmente la variable `myname=$(whoami)` por `bandit23` directamente en el script. Después, al ejecutarse el comando de encriptación con `md5sum`, podemos verificar en qué directorio se almacena la contraseña.

Este proceso nos permitirá identificar y recuperar la contraseña con facilidad.

---

**Nivel 23 > 24**
--------------------
### Password: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

![[Pasted image 20241029111440.png]]

![[Pasted image 20241023133548.png]]

Un programa se está ejecutando automáticamente a intervalos regulares a través de cron, el programador de trabajos basado en el tiempo. Para identificar qué comando se está ejecutando, revisamos la configuración en el directorio `/etc/cron.d/`.

Al analizar el código, vemos que el programa cambia al directorio `/var/spool/bandit24/foo`, donde recorre los archivos en busca de aquellos cuyo propietario sea `bandit23`. Si encuentra alguno, lo ejecuta como `bandit24` y, a continuación, lo elimina.

Para aprovechar este comportamiento, crearemos un script que contenga lo siguiente:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/(nuestro_directorio)/password
```

Este script leerá la contraseña de `bandit24` y la guardará en un archivo dentro de nuestro directorio temporal.

### Pasos

1. **Crear el script**: Guarda el código anterior en un archivo de texto en tu directorio.
2. **Crear el archivo vacío `password`**: Asegúrate de crear un archivo vacío llamado `password` en el mismo directorio.
3. **Asignar permisos**: Usa `chmod` para asegurarte de que tanto el script como el archivo `password` (y el directorio) tengan los permisos necesarios para evitar errores de permisos.
4. **Copiar el script**: Finalmente, copia el script al directorio donde se ejecuta el trabajo de cron, en este caso, `/var/spool/bandit24/foo`.

Siguiendo estos pasos, el trabajo cron ejecutará tu script como `bandit24`, y podrás capturar la contraseña en el archivo `password`.


---

**Nivel 24 > 25**
--------------------

### Password: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

![[Pasted image 20241024140230.png]]

En este nivel, un demonio está escuchando en el puerto 30002. Para obtener la contraseña de `bandit25`, necesitas enviarle dos cosas: la contraseña de `bandit24` y un código PIN secreto de 4 dígitos. La única forma de encontrar este PIN es mediante fuerza bruta, es decir, probando todas las 10,000 combinaciones posibles. No es necesario crear una nueva conexión para cada intento.

### Pasos

1. **Crear el diccionario de PINs**: Usaremos un bucle `for` para generar un archivo de combinaciones posibles de 4 dígitos:


```bash
for i in {0000..9999}; do echo "CONTRASEÑA_DE_BANDIT24 $i"; done > diccionario.txt
```

2. 

- Este comando genera cada posible combinación de PIN junto con la contraseña de `bandit24` y las guarda en un archivo `diccionario.txt`.
    
- **Establecer la conexión**: Luego, usaremos `nc` para conectarnos al puerto 30002 y probaremos cada línea del archivo `diccionario.txt`. Usando `cat` y un pipe, enviamos cada combinación al servicio escuchando en el puerto:

```bash
cat diccionario.txt | nc localhost 30002 | grep -vE "Wrong|Please enter"
```


1. El `grep -vE "Wrong|Please enter"` oculta las respuestas incorrectas, de modo que solo se muestre la contraseña correcta en el resultado.

Este enfoque permitirá iterar sobre todas las combinaciones generadas sin interrupciones y obtener la contraseña cuando el demonio acepte la combinación correcta.

---

**Nivel 25 y 26 > 27**
--------------------

### Password: s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ > 26
### Password: upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB > 27

![[Pasted image 20241029124110.png]]

![[Pasted image 20241024164557.png]]

![[Pasted image 20241024164745.png]]

![[Pasted image 20241027214417.png]]

Para acceder al nivel 27 desde bandit25, necesitaremos entender cómo funciona el shell asignado a `bandit26`. En lugar de `/bin/bash`, el shell asignado es `/usr/bin/showtext`, que ejecuta el siguiente script:

```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

Este script utiliza `more` para mostrar el contenido de `text.txt` en el directorio personal y luego ejecuta `exit 0`, lo cual cierra la conexión. Para evitar el cierre inmediato, aprovecharemos el comportamiento de `more`.

### Pasos

1. **Activar el modo de paginación en `more`**: Reducimos el tamaño de la terminal hasta hacer que `more` entre en modo de paginación, lo que permitirá interactuar con el archivo sin salir inmediatamente.
    
2. **Ingresar al editor**: Cuando estemos en el modo de paginación, presionamos `v` para abrir el archivo en el editor `vi`. Una vez dentro de `vi`, usaremos un truco para obtener un shell interactivo.
    
3. **Cambiar el tipo de shell y abrir una sesión**:
    
    - Presiona `Esc` y escribe `:set shell=/bin/bash` para definir el shell como `/bin/bash`.
    - Luego, escribe `:shell` y presiona `Enter` para obtener una shell interactiva de Bash.
4. **Acceder a la contraseña de bandit27**: Dentro de la nueva shell, puedes buscar el binario **setuid** que permite mostrar la contraseña de `bandit27`, similar a lo visto en niveles anteriores.

Este procedimiento permite acceder directamente al nivel 27 tras obtener la contraseña.


---

**Nivel 27 > 28**
--------------------
### Password: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

![[Pasted image 20241027220347.png]]

En este nivel, existe un repositorio Git disponible en `ssh://bandit27-git@localhost/home/bandit27-git/repo`, accesible a través del puerto `2220`. La contraseña para el usuario `bandit27-git` es la misma que la de `bandit27`.

### Pasos

1. **Clonar el repositorio**: Utilizaremos `git clone` para clonar el repositorio especificando el puerto `2220`:
```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```
**Leer el archivo `README`**: Una vez clonado el repositorio, navega hasta la carpeta y utiliza `cat` para leer el archivo `README`:
```bash
cd repo
cat README
```




---

**Nivel 28 > 29**
--------------------
### Password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

![[Pasted image 20241027220729.png]]

En este nivel, tenemos acceso a un repositorio Git ubicado en `ssh://bandit28-git@localhost/home/bandit28-git/repo` a través del puerto `2220`. La contraseña para el usuario `bandit28-git` es la misma que la de `bandit28`.

### Pasos

1. **Clonar el repositorio**: Clonamos el repositorio utilizando `git clone` y especificando el puerto `2220`:
```bash
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo

```
2. **Leer el archivo `README`**: Al hacer un `cat` al archivo `README`, veremos que contiene solo una serie de `xxxxxxx` en lugar de la contraseña real. Esto sugiere que hubo una modificación en el repositorio.

3. 1. **Investigar los cambios previos**:
    
    - Usamos `git log` para listar los cambios realizados en el repositorio.
    - Luego, con `git show <commit_id>`, examinamos cada cambio, lo cual nos permitirá ver el contenido previo del archivo `README` y encontrar la contraseña antes de que fuera reemplazada por `xxxxxxx`.

Al obtener la contraseña de esta manera, podremos avanzar al siguiente nivel.

---

**Nivel 29 > 30**
--------------------
### Password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

![[Pasted image 20241027221243.png]]

En este nivel, tenemos acceso a un repositorio Git en `ssh://bandit29-git@localhost/home/bandit29-git/repo`, a través del puerto `2220`. La contraseña para `bandit29-git` es la misma que para el usuario `bandit29`.

### Pasos

1. **Clonar el repositorio**: Utilizamos `git clone` especificando el puerto `2220`:
```bash
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
```

2. **Explorar el contenido de `README`**: Tras clonar el repositorio, hacemos un `cat` al archivo `README`, pero encontramos que no contiene la contraseña. Al revisar los cambios en los registros de `git log`, tampoco encontramos información relevante.
    
3. **Buscar en las ramas**:
    
    - Para comprobar si existen ramas adicionales, utilizamos `git branch -a`. Esto nos mostrará todas las ramas del repositorio, incluyendo cualquier rama remota.
    - Observamos que, además de la rama principal (`master`), existe una rama llamada `dev`.
4. **Cambiar a la rama `dev`**:
    
    - Cambiamos a la rama `dev` con el siguiente comando:
	```bash
	git checkout dev
	```
	- Dentro de la rama `dev`, volvemos a listar el archivo `README` usando `cat README`. Aquí encontramos una contraseña, que nos permitirá avanzar al siguiente nivel.


Esta serie de pasos nos lleva a descubrir la contraseña oculta en una rama diferente dentro del repositorio.


---

**Nivel 30 > 31**
--------------------
### Password: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy

![[Pasted image 20241027221752.png]]

En este nivel, accedemos a un repositorio Git ubicado en `ssh://bandit30-git@localhost/home/bandit30-git/repo` a través del puerto `2220`, utilizando la misma contraseña que para el usuario `bandit30`.

### Pasos

1. **Clonar el repositorio**: Usamos `git clone` especificando el puerto `2220`:
```bash
	git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
```
2. **Explorar el repositorio**: Al intentar los métodos anteriores como revisar el `README` y buscar en ramas o en el historial de commits, no encontramos la información necesaria.
    
3. **Buscar etiquetas con `git tag`**:
    
    - El comando `git tag` muestra una lista de etiquetas en el repositorio. Las etiquetas son referencias usadas para marcar puntos específicos en el historial, a menudo en versiones importantes o para señalar contenido relevante.
	```bash
    git tag
	```
	- Esto revela una etiqueta llamada `secret`.

4. **Ver el contenido de la etiqueta**:
    
    - Usamos `git show secret` para mostrar el contenido asociado a esta etiqueta:
        
        
		```bash
        git show secret
		```

        
    - Esto nos proporciona la contraseña que necesitamos para avanzar al siguiente nivel.
        

Esta estrategia nos permite descubrir la contraseña oculta en una etiqueta especial dentro del repositorio.

---

**Nivel 31 > 32**
--------------------
### Password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

![[Pasted image 20241027222323.png]]

En este nivel, accedemos a un repositorio Git ubicado en `ssh://bandit31-git@localhost/home/bandit31-git/repo` a través del puerto `2220`, utilizando la misma contraseña que para el usuario `bandit31`.

### Pasos

1. **Clonar el repositorio**: Usamos `git clone` para acceder al repositorio:
    
```bash
    git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
```

    
2. **Crear el archivo `key.txt`**: Creamos un archivo llamado `key.txt` y le añadimos el contenido requerido:
    
```bash
    echo "May I come in?" > key.txt
```
	
	
	
3. **Subir el archivo al repositorio**:
    
    - **Añadir el archivo**: Utilizamos `git add` para preparar el archivo para su confirmación. Dado que es un archivo nuevo, utilizamos el parámetro `-f` para forzar la inclusión:
        
	```bash
    git add -f key.txt
	```

        
    - **Hacer un commit**: Confirmamos el cambio con un mensaje descriptivo:
        
	```bash
	git commit -m "Add key.txt with required content"
	```

        
    - **Pushear a la rama principal**: Subimos el archivo a la rama `master` del repositorio:
        
	```bash
    git push -u origin master
	```

        
4. **Proporcionar la contraseña**: Al ejecutar el comando `git push`, se nos pedirá la contraseña del nivel. Ingresamos la contraseña correspondiente.
    
5. **Obtener la siguiente contraseña**: Después de haber subido el archivo, el sistema nos devolverá la contraseña para avanzar al siguiente nivel.
    

Este proceso nos permite completar la tarea de manera efectiva y obtener acceso al siguiente nivel.

---


**Nivel 32 > 33**
--------------------

![[Pasted image 20241027223740.png]]

En este último nivel, te encuentras en una shell conocida como la **UPPERCASE SHELL**, donde cualquier texto que escribas se convierte automáticamente en mayúsculas, y parece que las variables de entorno como `$HOME` o `$SHELL` se encuentran disponibles para su consulta. Esto nos da una pista sobre la naturaleza de esta shell especial.

Para escapar de esta shell, se puede utilizar la variable `$0`. En la mayoría de los entornos, `$0` contiene el nombre del ejecutable que lanzó el proceso actual. Sin embargo, en algunas shells especiales, como esta UPPERCASE SHELL, ejecutar `echo $0` puede mostrar el binario o script responsable de la shell misma.

Cuando ejecutas `$0`, se revela el nombre o la ruta de este programa y, en algunos casos, al escribir `$0`, es posible que el programa termine su ejecución y te devuelva a una shell convencional, como Bash. En este caso, el objetivo es que el UPPERCASE SHELL finalice, liberándote para iniciar una shell Bash normal, donde podrás consultar el texto final o contraseña del siguiente nivel usando un comando como `cat`.

En resumen, `$0` actúa aquí como una forma de **llamar al ejecutable actual** (la UPPERCASE SHELL) y, al invocarlo, provocar una interrupción o salida, que te permite recuperar el control.








