<h1>Laboratorio HackZones</h1>
<h1>Dificultad: Media</h1>
<h1>Vulnerabilidades: 
<li>Subida de archivos maliciosos para generar una reverse shell</li>
</h1>
<br><br>

<h2>Despleguamos el laboratorio "HackZones"</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 183406.png"/>

<h2>Realizamos un reconocimiento mediante la herramienta nmap para conocer puertos y servicios activos.</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 183500.png"/>

<h2>
<li>Puerto 22 abierto: ssh</li>
<li>Puerto 53 abierto: domain</li>
<li>Puerto 80 abierto: http</li>
</h2>

<h2>Si entramos al sitio web si nos vamos hasta abajo, encontramos un hosts el cual añadiremos a nuestro directorio /etc/hosts</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 184311.png"/>

<h2>Añadimos</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 184352.png"/>

<h2>Entramos al hosts añadido: hackzones.hl y encontramos esto</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 184837.png"/>

<h2>Realizamos un fuzzing de directorios</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185109.png"/>

<h2>Encontramos 2 directorios que llaman mas la atención, el directorio dashboard.html. Aquí tenemos la posibilidad de subir una foto a nuestro perfil</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185123.png"/>

<h2>Y el segundo directorio: uploads</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185131.png"/>

<h2>Como se mencionaba en el directorio: dashboard.html existe la función de subir archivos para añadir una foto de perfil </h2>
<h2>Una vez detectado esto vamos a subir un archivo malicioso que nos permita obtener una reverse shell, para la cual descargaremos y subiremos esto: </h2>
<h2>https://github.com/flozz/p0wny-shell/blob/master/shell.php</h2>
<h2>Descargamos y subimos.</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185142.png"/>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185408.png"/>

<h2>Nos vamos a uploads y ejecutamos.</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185419.png"/>

<h2>Shell web obtenida</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185650.png"/>

<h2>Ahora nos ponemos en escucha por netcat: nc -nlvp (PUERTO)</h2>
<h2>Y mandamos un reverse shell de la siguiente forma</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185815.png"/>

<h2>Reverse shell completada</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 185834.png"/>

<h2>Hacemos tratamiento de la TTY</h2>
<h2><li>script /dev/null -c bash</li></h2>
<h2><li>ctrl + z</li></h2>
<h2><li>stty raw -echo;fg</li></h2>
<h2><li>reset xterm</li></h2>
<h2><li>export SHELL=bash</li></h2>
<h2><li>export TERM=xterm</li></h2>

<h2>Ahora analizamos usuarios dentro del sistema y encontramos: mrrobot</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 190025.png"/>

<h2>Dentro del directorio /var/www/html/supermegaultrasecretofolder: se encuentra un script con contenido hexadecimal y al parecer ese codigo en hexadecimal concatena una palabra</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 190509.png"/>

<h2>Asi que vamos a descifrar ese codigo en hexadecimal.</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 191044.png"/>

<h2>Al parecer es la contraseña del usuario: mrrobot, probamos.</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 191123.png"/>


<h2>Una vez siendo el usuario mrrobot analizamos permisos sudo -l y encontramos el binario /usr/bin/cat</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 191324.png"/>

<h2>Explorando un poco los directorios encuentro que en la ruta /opt existe un archivo llamado SistemUpdate, el cual mediante el binario /usr/bin/cat vamos a leer el contenido</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 191518.png"/>

<h2>Dentro de ese archivo, se filtra la contraseña de root : rooteable</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 191540.png"/>

<h2>Ponemos la contraseña</h2>
<img width="956" height="776" src="../HackZones/image/Captura de pantalla 2026-09-22 191617.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h1>Creditos a: vareCruzz</h1>
<h2>TE AMO LIZETTE, TE VOY A HACKEAR EL CORAZON <3</h2>





