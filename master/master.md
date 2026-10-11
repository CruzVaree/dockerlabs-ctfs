<h1>Laboratorio Master</h1>
<h1>Dificultad: Media</h1>
<h1>Vulnerabilidades: Explotación de un plugin vulnerable dentro de wordpress: CVE-2024-27956</h1>
<br><br>

<h2>Despleguamos el laboratorio "Master"</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 170025.png"/>

<h2>Realizamos un escaneo de nmap como parte del reconocimiento de puertos y servicios activos</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 170208.png"/>

<h2><li>Puerto 80 abierto: HTTP</li></h2>

<h2>Entramos al sitio web mediante el navegador</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 170231.png"/>

<h2>Explorando el sitio encontre en la parte de abajo, un plugin con el cual esta hecho el sitio web</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 170711.png"/>

<h2>Realizando una búsqueda de vulnerabilidades relacionadas con ese plugin encontramos el siguiente CVE: <br>
CVE-2024-27956</h2>
<h2>El CVE-2024-27956 es una vulnerabilidad de inyección SQL (CWE-89) que afecta al complemento WordPress Automatic</h2>

<h2>Buscando por github un exploit para este CVE y encontramos lo siguiente: </h2>
<h2>https://github.com/diego-tella/CVE-2024-27956-RCE</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 171513.png"/>

<h2>Copiamos o nos descargamos el exploit y ejecutamos.</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 172011.png"/>

<h2>Lo que realizo este exploit a pocas palabras fue crearnos un usuario con privilegios de administrador realizando una consulta SQL para registrar ese usuario dentro de la base de datos.</h2>

<h2>Ahora nos vamos al panel /wp-admin e iniciamos sesion</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 172819.png"/>

<h2>Hemos entrado como administrador</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 173442.png"/>

<h2>Nos vamos a la siguiente seccion</h2>
<h2>Appearance > theme file editor > functions.php</h2>
<img src="../master/image/Captura de pantalla 2026-10-10 173736.png"/>
<h2>Aqui pegamos dentro de functions.php una reverse shell hecha en php</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 173844.png"/>

<h2>Posteriormente nos ponemos en escucha por netcat: nc -nlvp (PORT)</h2>
<h2>Ejecutamos la reverse shell dando click en "Upload file"</h2>
<h2>Reverse shell ganada</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 173903.png"/>

<h2>HACEMOS TRATAMIENTO DE LA TTY</h2>
<h2><li>script /dev/null -c bash</li></h2>
<h2><li>ctrl + z</li></h2>
<h2><li>stty raw -echo;fg</li></h2>
<h2><li>reset xterm</li></h2>
<h2><li>export SHELL=bash</li></h2>
<h2><li>export TERM=xterm</li></h2>

<h2>Despues analizamos permisos sudo -l y encontramos el binario /usr/bin/php el usuario "pylon" puede ejecutar este binario</h2>
<h2>Nos aprovechamos de este binario y hacemos un pivoting de usuario lanzando una bash como "pylon"</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 174312.png"/>

<h2>Somos el usuario pylon</h2>
<h2>Volvemos a analizar permisos sudo -l y encontramos que el usuario mario tiene puede ejecutar el binario <br> 
/bin/bash en el script /home/mario/pingusorpresa.sh, ejecutamos el script y añadimos lo siguiente: <br> 
a[$(/bin/bash 2>&)]+40 <br>
dentro del script añadimos eso ya que aprovechamos un operador usado en el codigo "eq"
</h2>

<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 175012.png"/>
<h2>Somos el usuario mario, ahora volvemos a analizar permisos sudo -l</h2>
<h2>Y volvemos a aprovechar el operador "eq"</h2>
<img width="956" height="776" src="../master/image/Captura de pantalla 2026-10-10 175305.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h2>Creditos: vareCruzz</h2>
<h2>TGl6ZXR0ZSB0ZSBhbW8gbXVjaG8gPDM=</h2>



