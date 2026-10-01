<h1>Laboratorio Apolos</h1>
<h1>Dificultad: Medio</h1>
<h1>Vulnerabilidades: Explotación de Inyección SQL</h1>
<br><br>

<h2>Despleguemos el laboratorio "Apolos"</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 194114.png"/>

<h2>Realizamos un escaneo de nmap como parte del reconocimiento de puertos y servicios</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 200339.png"/>

<h2>
<li>Puerto 80 abierto: HTTP</li>
</h2>

<h2>Entramos al sitio web mediante el navegador</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 200519.png"/>

<h2>Tenemos la opcion de crearnos una cuenta nos registramos e iniciamos sesión</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 200349.png"/>

<h2>Se a detectado que dentro de la sección "mi carrito(mycart.php)" existe una vulnerabilidad que nos permite explotar una inyeccion SQL</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 200449.png"/>

<h2>Mediante la herramienta SQL map vamos a explotar esta vulnerabilidad</h2>
<h2>1. Vamos a encontrar las bases de datos existentes</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 200846.png"/>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 200858.png"/>

<h2>2. Ya que conocemos las bases de datos vamos a ver todo el contenido de la DB "apple_store"</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 201240.png"/>
<h2>Principalmente nos interesa la tabla: users</h2>
<h2>Esta tabla almacena los usuarios con sus respectivas contraseñas en formato de hash</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 201252.png"/>

<h2>Vamos a seleccionar cualquier hash para identificar a que tipo pertenece</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 201541.png"/>

<h2>El hash que nos interesa es el del usuario admin asi que mediante la herramienta de "john" vamos a descubrir la contraseña</h2>
<h2>Guardamos el hash usando el editor nano</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 202028.png"/>

<h2>Se a descifrado la contraseña de admin, así que vamos a iniciar sesión con ese usuario</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 202045.png"/>

<h2>Entramos al panel de administrador y nos vamos a configuración</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 202100.png"/>

<h2>Dentro de configuración tenemos la opcion de subir un archivo, asi que se subira un archivo malicioso que nos de una reverse shell.</h2>
<h2>Se subira la siguiente reverse shell: https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php</h2>
<h2>PUNTO IMPORTANTE: cambiar la extension de .php a .phtml</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 202223.png"/>


<h2>Ya que se subio correctamente el archivo, encontramos un directorio /uploads donde se almaceno la reverse shell</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 202258.png"/>


<h2>Nos ponemos en escucha por netcat de acuerdo al puerto puesto en la reverse shell:<br>
nc -nlvp (PUERTO)</h2>
<h2>Ejecutamos</h2>
<h2>Reverse shell completada</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-29 202311.png"/>

<h2>HACEMOS TRATAMIENTO DE LA TTY</h2>
<h2><li>script /dev/null -c bash</li></h2>
<h2><li>ctrl + z</li></h2>
<h2><li>stty raw -echo;fg</li></h2>
<h2><li>reset xterm</li></h2>
<h2><li>export SHELL=bash</li></h2>
<h2><li>export TERM=xterm</li></h2>


<h2>Una vez dentro analizamos los usuarios dentro del sistema</h2>
<h2>Usuario encontrado: luisillo_o</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 154624.png"/>

<h2>Para ser el usuario luisillo_o vamos a descifrar su contraseña mediante un diccionario y el siguiente script que descargaremos en nuestra maquina atacante:</h2>
<h2>https://github.com/CruzVaree/scripts_hacking/blob/main/forceBrute.sh</h2>
<h2>Y nos pasamos el diccionario rockyou.txt</h2>

<h2>Levantamos un servidor en python desde la maquina atacante: python3 -m http.server 80</h2>
<h2>Desde la maquina victima nos movemos al directorio /tmp, realizamos un wget para descargar el script y el diccionario</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 155306.png"/>

<h2>Una vez pasados a la maquina victima, damos permisos de ejecución y ejecutamos</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 162412.png"/>

<h2>Hemos obtenido la contraseña, entramos por el usuario luisillo_o</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 162453.png"/>

<h2>Usuario luisillo_o obtenido</h2>
<h2>Si visualizamos el contenido del directorio cat /etc/shadow encontramos un hash que puede contener la contraseña del usuario root</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 163110.png"/>

<h2>Nos copiamos el hash y mediante la herramienta de john desciframos la contraseña</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 163503.png"/>


<h2>Probamos la contraseña con el usuario root</h2>
<img width="1336" height="879" src="/Apolos/image/Captura de pantalla 2026-09-30 163530.png"/>

<h2>USUARIO ROOT OBTENIDO :)</h2>


<h1>Creditos a: vareCruzz</h1>
<h1>Agradecimientos a mi novia que me motiva a no rendirme y a mejorar <3. La amo muchote</h1>






