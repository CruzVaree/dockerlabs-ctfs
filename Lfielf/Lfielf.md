<h1>Laboratorio Lfi.elf</h1>
<h1>Dificultad: Dificil</h1>
<h1>Vulnerabilidades: LFI (Local File Inclusion).
</h1>
<br>
<h2>Despleguamos el laboratorio "Lfi.elf"</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 191501.png"/>

<h2>Realizamos un escaneo de nmap para conocer servicios activos y puertos abiertos</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 191813.png"/>

<h2>
  <li>Puerto 80 abierto: http</li>
</h2>

<h2>Accedemos al sitio web mediante el navegador</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 191843.png"/>

<h2>No encontramos nada interesante dentro de la web, así que realizamos un fuzzing de directorios</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 192217.png"/>

<h2>Encontramos varias rutas probablemente en las rutas de .php al final puede que se acontezca un LFI, así que vamos a empezar a probar por la ruta index.php</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 192907.png"/>

<h2>Haciendo fuzzing al parámetro vulnerable vemos que index.php con el parámetro search nos permitirá ver el directorio /etc/passwd, así que tenemos un LFI</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 192929.png"/>

<h2>Ya que tenemos un LFI podemos aprovecharnos de esto para ejecutar y leer archivos internos de la maquina, asi que usaremos la siguiente herramienta para ejecutar archivos: </h2>
<h2>https://github.com/synacktiv/php_filter_chain_generator/blob/main/php_filter_chain_generator.py</h2>

<h2>Vamos a confirmar que podamos ejecutar archivos, asi que vamos a mandar un phpinfo y ver si el sitio web lo interpreta</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 193123.png"/>
<h2>Copiamos a partir del: php://filter hasta el final</h2>
<h2>http://172.17.0.2/index.php?search= AQUI PEGAMOS LO GENERADO POR LA HERRAMIENTA PHP FILTER CHAIN</h2>

<h2>Pegamos esto dentro del navegador y como vemos se interpreto</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 193239.png"/>

<h2>Posteriormente vamos a obtener una reverse shell mediante los siguientes pasos: </h2>
<h2>1-Nos creamos una reverse shell (es importante que solo lo pongan como rs.sh para evitar que a la hora de generar con la herramienta php filter chain no sea tan largo lo que genere).</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 195612.png"/>

<h2>2-Levantamos un servidor en python</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 195348.png"/>

<h2>3-Ya que tenemos la reverse shell y el servidor de python, vamos a generar una petición que haga un curl para obtener y ejecutar esa reverse shell</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 195833.png"/>

<h2>4-Nos ponemos en escucha por netcat de acuerdo al puerto declarado en la reverse shell: 
nc -nlvp (PORT)</h2>
<h2>5-Pegamos lo generado en el navegador</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 200529.png"/>


<h2>REVERSE SHELL COMPLETADA</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 200556.png"/>

<h2>Hacemos tratamiento de la TTY</h2>
<h2><li>script /dev/null -c bash</li></h2>
<h2><li>ctrl + z</li></h2>
<h2><li>stty raw -echo;fg</li></h2>
<h2><li>reset xterm</li></h2>
<h2><li>export SHELL=bash</li></h2>
<h2><li>export TERM=xterm</li></h2>

<h2>Analizando y explorando directorios dentro de la ruta /var/www se encuentra un directorio /.secret_www-data</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 200743.png"/>

<h2>Dentro de /.secret_www-data encontramos otro directorio llamado /.passwd y encontramos un passwords.txt y contiene la contraseña del usuario lin</h2>
<h2>Accedemos con esas credenciales.</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 200912.png"/>

<h2>Usuarios lin obtenido</h2>
<h2>En la ruta /home/lin encontramos un script en python llamado: sistem.py</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 200959.png"/>

<h2>Vemos el contenido del script</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 201046.png"/>

<h2>A lo que vemos resumidamente el script ejecuta un script y unas funciones dentro del codigo de acuerdo a la opción que elijamos, ejecuta un script llamado: subtheads.py</h2>

<h2>Buscamos el script "subtheads.py" y analizamos su codigo</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 201625.png"/>

<h2>En resumen: el código es un pequeño lanzador que intenta ejecutar /tmp/script.sh como administrador mediante sudo. El comportamiento real —y si es peligroso— depende de qué contiene /tmp/script.sh</h2>


<h2>Asi que vamos a la ruta /tmp y creamos el "script.sh" con el siguiente contenido</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 201740.png"/>

<h2>Damos permisos de ejecución: chmod +x script.sh</h2>
<h2>Volvemos a la ruta /home/lin y ejecutamos el script y elegimos la opción 3 para ejecuta el script: subtheads.py y ejecute a la vez el script.sh</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 201839.png"/>

<h2>Mediante ls -al /bin/bash veremos si ya tenemos privilegios de root</h2>
<h2>-rwsr-xr-x 1 root root 1446024 Mar 31 2024 /bin/bash</h2>
<img width="956" height="776" src="../Lfielf/image/Captura de pantalla 2026-09-23 202044.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h1>Creditos a: vareCruzz</h1>
<h1>Amo mucho y adoro mucho a mi noviaaa <3</h1>

