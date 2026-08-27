<h1>Laboratorio Hidden</h1>

<h2>Despleguamos el laboratorio Hidden</h2>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 210923.png"/>

<h3>Realizamos un escaneo de nmap para encontrar versiones y puertos abiertos dentro del laboratorio</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 211058.png"/>
<li>Puerto 80</li>

<h3>direccion URL del servicio tenemos un hostsDiscovery asi que agregamos a nuestro archivo:  /etc/hosts</h3>
<h3>172.17.0.2 hidden.lab</h3>
<h3>http://hidden.lab/</h3>

<h3>Entramos a la url mediante el navegador</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 211133.png"/>

<h3>Explorando el sitio web no encontramos nada interesante asi que realizaremos un fuzzing de directorios</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 211342.png"/>

<h3>Tampoco encontramos nada interesante asi que realizare un fuzzing de subdominios</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 211522.png"/>

<h3>Encontramos el subdominio: dev</h3>
<h3>vamos a agregarlo a nuestro archivo: /etc/hosts</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 211759.png"/>

<h3>Entramos al subdominio y encontramos esto una función del sitio web de subir un pdf, para ganar acceso remoto usaremos esta siguiente reverse shell:</h3>
<h3>https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php</h3>

<h3>Para que funcione y se ejecute este archivo cambiaremos la extension de .php a .phtml y subimos</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 212121.png"/>

<h3>Una vez subido, nos vamos al directorio /uploads y antes de ejecutar nuestro archivo/script nos ponemos en escucha por netcat: nc -nlvp [PORT]</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 212140.png"/>

<h3>Reverse shell completada :)</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 212149.png"/>
<h3>Hacemos el tratamiento de la TTY</h3>
<li>script /dev/null -c bash</li>
<li>stty raw -echo;fg</li>
<li>reset xterm</li>
<li>export SHELL=bash</li>
<li>export TERM=xterm</li>

<h3>Somos el usuario www-data haremos un pivoting de usuarios para eso obtendremos las credenciales/contraseñas de los usuarios</h3>
<h3>Desde nuestra maquina atacante descargaremos este script: https://github.com/nohh022/bruteForce </h3>

<h3>En la mquiana atacante vamos a codificar el rockyou.txt para pasarlo a la maguina victima</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 213250.png"/>

<h3>En la maquina victima nos movemos al directorio /tmp y decodificamos el rockyou.txt</h3>
<h3>echo '[CODIGO EN  BASE64]' | base64 -d > rockyou_short.txt </h3>

<h3>Haremos lo mismo con el script codificamos el script</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 213835.png"/>

<h3>Hacemos lo mismo desde la maquina victima</h3>
<h3>echo 'CODIGO EN BASE64' | base64 -d > force.sh</h3>

<h3>Una vez pasado a la maquina victima el diccionario y el script, damos permisos de ejecucion al script: chmod +x force.sh</h3>
<h3>Analizamos que usuario atacaremos: cafetero</h3>
<h3>EJECUTAMOS</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 213957.png"/>

<h3>Credenciales obtenidas</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 214209.png"/>

<h3>Una vez siendo el usuario cafetero hacemos un: script /dev/null -c bash</h3>
<h3>Analizamos los permisos sudo -l y encontramos que john tiene el binario /usr/bin/nano</h3>
<h3>Para ser ese usuario haremos lo siguiente: </h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 214623.png"/>
<h3>Ejecutamos</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 214607.png"/>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 214654.png"/>

<h3>Somos el usuario john y haremos un: script /dev/null -c bash</h3>
<h3>Analizamos permisos sudo -l y encontramos que el usuario: bobby tiene el binario: /usr/bin/apt</h3>
<h3>Haremos otro pivoting hacia el usuario: bobby mediante lo siguiente</h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 215831.png"/>

<h3>Una vez siendo el usuario: bobby</h3>
<h3>Analizamos permisos sudo -l y encontramos que root tiene el siguiente binario: /usr/bin/find</h3>
<h3>Para ser root haremos lo siguiente: </h3>
<img width="956" height="776" src="../hidden/image/Captura de pantalla 2026-08-24 220154.png"/>

<h3>ROOT OBTENIDO :)</h3>

<h1>CREDITOS A: vareCruzz</h1>
























