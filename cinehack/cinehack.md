<h1>Laboratorio "cineHack"</h1>

<h2>Despleguamos el laboratorio cinehack</h2>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 165405.png"/>

<h3>Realizamos un escaneo de nmap para conocer puertos abiertos y servicios activos</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 165452.png"/>

<h3>Entramos al sitio web a través del navegador usando la dirección ip</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 165642.png"/>

<h3>Encontramos esto, al parecer cinema DL es dominio el cual agregaremos en nuestro /etc/hosts</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 165654.png"/>

<h3>Entramos al dominio y encontramos se visualiza esto</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 165816.png"/>

<h3>Entramos a la pelicula "El tiempo que tenemos" y vamos a realizar una reserva de asientos</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 165946.png"/>

<h3>No encontramos nada interesante a la hora de realizar la reserva</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 172012.png"/>

<h3>Por lo tanto capturaremos la petición con burpsuite para explorar mas a fondo como se tramita esto.</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 171945.png"/>

<h3>Al parecer encontramos un parametro oculto llamado "problem_url"</h3>
<h3>Mediante ese parametro nos aprovecharemos de una vulnerabilidad</h3>
<li>SSRF: Es una vulnerabilidad de seguridad que permite a un atacante obligar a un servidor web a enviar peticiones HTTP a destinos no autorizados, como servicios en la red interna o redes extermas</li>

<h3>Nos levanteremos un servidor en python para obtener a traves del parametro "problem_url" una reverse shell hecha en php</h3>
<h3>Creamos la reverse shell usaremos esta: https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php</h3>
<h3>Levantamos el servidor en python</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 172934.png"/>
<h3>Explotamos la vulnerabilidad SSRF para obtener la reverse shell mediante nuestro servidor en python</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 172953.png"/>
<h3>Hemos logrado pasar la reverse shell</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 173011.png"/>

<h3>Ahora deberemos buscar donde se almaceno esa reverse shell</h3>
<h3>La cual se almaceno en: http://cinema.dl/andrewgarfield</h3>
<h3>Nos ponemos en escucha por netcat nc -nlvp [PUERTO]</h3>
<h3>Ejecutamos la reverse shell</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 173040.png"/>


<h3>Reverse shell completa</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 173107.png"/>

<h3>Hacemos el tratamiento de la TTY</h3>
<li>script /dev/null -c bash</li>
<li>ctrl + Z</li>
<li>reset xterm</li>
<li>export TERM=xterm</li>
<li>export SHELL=bash</li>

<h3>Nos vemos a la ruta cd /var/www/cinema.dl/andrewgarfield</h3>
<h3>1. Haremos un sudo -l para analizar binarios sudo</h3>
<h3>2. Encontramos un binario que se puede ejecutar mediante el usuario "boss"</h3>
<h3>3. Con la reverse shell que ya tenemos almacenada dentro de la ruta la copearemos y cambiaremos el puerto de la nueva copia de la reverse shell</h3>
<h3>4. Ejecutamos la reverse shell copia mediante el binario /bin/php y nos pondremos en escucha por netcat mediante el otro puerto destinado</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 174133.png"/>
<h3>Reverse shell obtenida con el usuario boss</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 174150.png"/>

<h3>Al conectarnos con el usuario boss existe un script que nos expulsan cada minuto, por lo que se deduce que hay un crontab ejecutandose</h3>
<h3>Script que cierra la sesion de boss: </h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 174245.png"/>

<h3>Intentamos buscar a la ruta cd /var/spool/cron encontramos un direcotorio llamado crontabs a la cual no tenemos acceso mediante el usuario www-data</h3>
<h3>Ejecutaremos la reverse shell como el usuario boss para ver el contenido de ese directorio</h3>
<h3>Tendremos que ver lo que esta adentro del directorio rápido antes que nos cierren la sesión y cuando veamos lo que hay adentro cerramos la reverse shell 
antes que la cierren para evitar que igual nos cierren con www-data</h3>
<h3>visualizamos esto: </h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 174846.png"/>

<h3>Encontramos un script que se ejecuta cada minuto, pero como bien hemos visto que tambien esta ejecutando a la vez el /tmp/script.sh</h3>
<h3>Asi que con el usuario www-data nos crearemos un script que nos eleve priveligios y lo guardaremos con el nombre "script.sh"</h3>
<h3>echo 'chmod u+s /bin/bash' > script.sh</h3>
<h3>chmod +x script.sh</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 175231.png"/>

<h3>Esperaremos un minuto a que se ejecute el script</h3>
<h3>mediante ls -al /bin/bash esperaremos que bash tenga permisos SUID</h3>
<h3>-rwsr-xr-x 1 root root 1446024 Mar 31  2024 /bin/bash</h3>
<h3>Posteriormente una vez que la bash tenga permisos SUID haremos bash -p</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 175305.png"/>

<h3>ROOT OBTENIDO :)</h3>
<h3>FLAG:</h3>
<img width="956" height="776" src="../cinehack/images/Captura de pantalla 2026-08-04 175330.png"/>

<h1>CREDITOS A: vareCruzz</h1>




