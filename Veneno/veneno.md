<h1>Laboratorio "Veneno"</h1>
<h2>Vulnerabilidades:
<li>LFI: Vulnerabilidad de seguridad informática que permite a un atacante leer o ejecutar archivos internos de un servidor</li>
<li>RCE: Ejecucion remota de comandos</li>
<li>Log poisoning: Vulnerabilidad en la que atacante inyecta código malicioso en los archivos de registro (logs)</li>
</h2>
<h2>Dificultad: Media</h2>
<br><br>

<h2>Desplegamos el laboratorio veneno</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 122147.png" />

<h2>Realizamos un escaneo de nmap como parte del reconocimiento para conocer puertos y servicios</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 122459.png" />
<h2>
<li>Puerto 22 abierto: SSH</li>
<li>Puerto 80 abierto: HTTP</li>
</h2>

<h2>Entramos al sitio web que corre en el laboratorio y vemos una plantilla de apache predeterminada</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 122522.png" />

<h2>No hay nada interesante dentro del sitio web, asi que vamos a hacer un fuzzing de directorios</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 122831.png" />

<h2>Entramos a el directorio /problems.php y encontramos lo siguiente</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 123226.png" />

<h2>Probablemente el directorio /problems.php sea vulnerable a un LFI, asi que vamos a encontrar el parametro que nos permita explotar esta vulnerabilidad</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 124100.png" />

<h2>Se encuentra el parámetro vulnerable: backdoor</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 124143.png" />

<h2>En efecto nos enfrentamos a un LFI, vamos a ver si a partir del LFI podemos explotar un log poisoning a través de la ruta /var/log/apache2/access.log, si es visible este directorio podemos explotar un log poisoning </h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 131317.png" />

<h2>Y como se muestra en la imagen es visible los logs, así que vamos a inyectar código para obtener un RCE</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 131317.png" />

<h2>Mediante curl lanzaremos un comando "id"</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 163509.png" />
<h2>Actualizamos/recargamos el sitio donde se ve el access.log</h2>
<h2>El comando se ejecuto.</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 163620.png" />

<h2>Ahora procedemos a obtener una reverse shell</h2>
<h2>1-Descargaremos esta shell web: https://github.com/flozz/p0wny-shell/blob/master/shell.php</h2>
<h2>2-Levantamos un servidor en python para subirla en el directorio /uploads: python3 -m http.server 80</h2>
<h2>3-Ahora hacemos un curl con la siguiente peticion</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 165546.png" />
<h2>4-Actualizamos/recargamos el sitio donde se ve el access.log</h2>
<h2>Checamos que se allá subido a el directorio /uploads anteriormente encontrado en el fuzzing</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 165645.png" />

<h2>Le damos click a la webshell que subimos y se ejecutara</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 165818.png" />

<h2>Ya que tenemos la webshell nos lanzamos una reverse shell a nuestra maquina atacante</h2>
<h2>Desde nuestra maquina atacante nos ponemos en escucha por netcat: nc -nlvp (PORT)</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 165850.png" />

<h2>Reverse shell completada</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 165857.png" />

<h2>Hacemos tratamiento de la TTY</h2>
<h2>
<li>script /dev/null -c bash</li>
<li>crtl + z</li>
<li>stty raw -echo;fg</li>
<li>reset xterm</li>
<li>export SHELL=bash</li>
<li>export TERM=xterm</li>
</h2>

<h2>Analizando un poco los directorios encontramos esto</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 170332.png" />

<h2>Nos brinda un pista de donde esta la contraseña de este usuario</h2>
<h2>Buscando entre directorios encontré la contraseña del usuario carlos</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 170520.png" />

<h2>USUARIO CARLOS OBTENIDO</h2>
<h2>Analizamos el directorio /home/carlos y encontramos muchas carpetas</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 170548.png" />

<h2>Para ver el contenido de todas los directorios lanzaremos el siguiente comando: ls -alR</h2>
<h2>Y en la carpeta 55 encontramos una imagen jpg</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 170611.png" />

<h2>Entramos a la carpeta55 y levantamos un servidor en python para pasarnos la imagen a nuestra maquina atacante y analizarla mejor</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 170713.png" />

<h2>Hacemos un wget para descarga la imagen</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 170831.png" />

<h2>Mediante la herramienta exiftool analizaremos los datos no visibles de la imagen</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 173746.png" />

<h2>Encontramos la palabra: pingui1730</h2>
<h2>Esa palabra puede ser la contraseña de root asi que probamos</h2>
<img width="1336" height="879" src="/Veneno/image/Captura de pantalla 2026-09-19 173839.png" />


<h2>ROOT OBTENIDO :)</h2>
<br><br>
<h1>Creditos a: vareCruzz</h1>
<h1>Si mi corazón fuera un servidor, tú serías mi RCE, porque desde que llegaste tienes acceso total a todo lo que siento: para mi novia Lizette <3</h1>

















