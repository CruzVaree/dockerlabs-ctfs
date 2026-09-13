<h1>Laboratorio HackTheHeaven</h1>
<h1>Dificultad: Dificil</h1>
<h1>Vulnerabilidades: Explotación de un LFI/Path Traversal hacia RCE</h1>
<br><br>
<h1>PASO IMPORTANTE ANTES DE DESPLEGAR LA MAQUINA</h1>
<h2>Dentro del auto_deploy.sh se encontro que: Internamente se está utilizando IPv6 por defecto esto provoca que internamente el uso de IPv4 quede inservible, esto afecta a cierta parte del laboratorio con respecto a la intruccion. De la siguiente forma:
<br><br>
curl http://localhost:9999
<br>
Acceso denegado.
<br><br>
curl http://127.0.0.1:9999
<br>
curl: Failed to connect to 127.0.0.1 port 9999 after 0 ms: Couldn't connect to server
</h2>
<br><br>
<h2>Para corregir estos errores añadimos en la linea 68 de auto_deploy.sh</h2>
<h2>Aqui:</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-13 122140.png"/>
<h2>Añadimos esto (-d --sysctl net.ipv6.conf.all.disable_ipv6=1):</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-13 122236.png"/>
<h2>Guardamos cambios e iniciamos el laboratorio</h2>

<h2>DESPLEGUAMOS EL LABORATORIO "HackTheHeaven"</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 181626.png"/>

<h2>Realizamos un escaneo de nmap para reconocer servicios activos y puertos</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 181731.png"/>

<h2>Dentro del escaneo encontramos un sitio web. Vamos a ingresar a el mediante el navegador y encontramos esto: </h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 181754.png"/>

<h2>No encontramos nada interesante dentro del sitio así que continuamos con un fuzzing de directorios</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182135.png"/>

<h2>Encontramos la direccion info.php la cual sirve para mostrar en el navegador toda la información detallada sobre la configuración de PHP en el servidor web mediante la función.</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182147.png"/>

<h2>Analizando la información se encontró lo siguiente: </h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182216.png"/>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182355.png"/>
<h2><li>disable_functions: no value: Permite funciones peligrosas que permiten ejecutar comandos en el servidor (como system, exec, shell_exec o passthru)</li></h2>
<h2><li>file_uploads: On: Permite la subida de archivos</li></h2>


<h2>Analizamos la ruta idol.html</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182430.png"/>
<h2>Encontramos un botón llamado DarkNet que nos redirige a una ruta</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182456.png"/>
<h2>Nos manda a la estar ruta que indica algo "Error: No se ha especificado un archivo para incluir" esto significa que aquí se puede llegar a acontecer un LFI y a la vez la ruta termina en .php</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 182549.png"/>


<h2>Para reconocer si existe realmente un LFI vamos a encontrar ese parametro vulnerable que nos dejara acceder a los archivos locales del servidor</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 183334.png"/>

<h2>Encontramos el parámetro "filename". Visualizamos desde el navegador</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 183410.png"/>

<h2>Nos confirma aquí el LFI, así que vamos a probar distintos tipos de bypass para acontecer el LFI</h2>
<h2>Probamos el siguiente y no aparece nada</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 183451.png"/>
<h2>Probamos el siguiente y en efecto funciona el LFI</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-11 183634.png"/>

<h2>Con la explotación del LFI, el directorio visible de info.php, el reconocimiento de funcionalidades activas dentro del info.php pude deducir que podemos explotar un: LFI a RCE</h2>
<h2>Asi que usaremos el siguiente script de: https://hacktricks.wiki/es/pentesting-web/file-inclusion/lfi2rce-via-phpinfo.html</h2>
<h2>Ahi mismo en hakctricks viene toda la documentación acerca del script a utilizar</h2>
<h2>En caso de que no se encuentre el script lo subi a mi github (CREDITOS A hacktricks): https://github.com/CruzVaree/scripts_hacking/blob/main/enumerarApi.py</h2>

<h2>°Dentro del script encontrado en mi github ya esta modificado todo de acuerdo a los parametros necesarios para explotar el laboratorio!</h2>

<h2>PARAMETROS DEL SCRIPT ORIGINAL QUE SE CAMBIARON: </h2>
<h2>ruta del info.php</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 145354.png"/>
<h2>Ruta donde se acontece el LFI</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 145451.png"/>
<h2>Se añadio el payload (reverse shell)</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 145615.png"/>
<h2>Se añadio esto</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 151807.png"/>

<h2>COMO MENCIONE YA EL SCRIPT QUE ESTA EN MI GITHUB YA VIENE MODIFICADO CON ESOS ASPECTOS (CREDITOS a hacktricks)</h2>

<h2>Nos ponemos en escucha por netcat nc -nlvp (PORT) de acuerdo al puerto indicado en el payload. Ejecutamos el script una vez obtenido</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 152418.png"/>
<h2>Reverse shell completada :)</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 152437.png"/>
<h2>HACEMOS TRATAMIENTO DE LA TTY</h2>
<h2><li>script /dev/null -c bash</li></h2>
<h2><li>ctrl + z</li></h2>
<h2><li>stty raw echo;fg</li></h2>
<h2><li>reset xterm</li></h2>
<h2><li>export SHELL=bash</li></h2>
<h2><li>export TERM=xterm</li></h2>


<h2>Dentro del directorio /home se encontro una posible contraseña: "megustaelfallout" asi que lo probaremos con los diferentes usuarios del sistema</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 154043.png"/>

<h2>Somos el usuario xerosec y posteriormente analizamos permisos sudo</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 154311.png"/>
<h2>Encontramos que mario puede ejecutar un script como permisos sudo dentro del directorio /tmp/script.py</h2>
<h2>Analizamos el contenido de "script.py" y podemos explotar un Python Library Hijacking aprovechándonos de la libreria hashlib</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 154336.png"/>

<h2>Posteriormente creamos un script de python llamado igual que la librería hashlib.py con el siguiente contenido adentro: </h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 154450.png"/>

<h2>Guardamos y ejecutamos el script.py</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 154657.png"/>

<h2>Y hemos conseguido ser el usuario mario</h2>
<h2>Analizamos el directorio /home/mario y encontramos un .txt</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 154749.png"/>

<h2>Nos indica que existe un server del propietario s4vitar corriendo dentro del laboratorio así que analizamos los procesos ejecutandose</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-12 155526.png"/>
<h2>Se esta ejecutando un server localhost por el puerto 9999</h2>
<h2>Así que realizamos una petición mediante curl</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-13 123538.png"/>

<h2>Nos da como respuesta que necesitamos pasarle un comando así que usaremos la ruta que se nos había dicho en el .txt</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-13 124023.png"/>
<h2>Vemos que se pude ejecutar el comando id, asi que pasamos una reverse shell url encodenada para que funcione pero antes nos ponemos en escucha por netcat 
nc -nlvp (PORT)</h2>

<h2>Reverse shell obtenida</h2> 
<h2>Una vez siendo el usuario s4vitar, analizamos permisos sudo y encontramos el binario /usr/bin/xargs que es vulnerable a una escalada de privilegios y escalaremos de la siguiente forma como se muestra en la imagen :)</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-13 124248.png"/>

<h2>FLAG ROOT</h2>
<img width="956" height="776" src="../HackTheHeaven/image/Captura de pantalla 2026-09-13 124812.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h1>Creditos a: vareCruzz</h1>
<h1>Mi novia Lizette es la mejor, todos mis logros son dedicados a ella porque me motiva a mejorar. TE AMO LIZETTE <3</h1>
