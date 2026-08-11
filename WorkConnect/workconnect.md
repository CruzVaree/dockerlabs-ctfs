<h1>Laboratorio WorkConnect</h1>

<h2>Despleguamos el laboratorio workConnect</h2>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 185438.png"/>

<h3>Realizamos un escaneo de nmap como parte del reconocimiento</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 185617.png"/>
<li>Puerto 8000</li>

<h3>Entramos al sitio web mediante el navegador poniendo la dirección ip</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 190827.png"/>

<h3>Nos registramos e iniciamos sesión en el sitio</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 190857.png"/>

<h3>Cuando iniciamos sesión veremos esto un perfil que nos permite cambiar nuestra foto de perfil mediante una url</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 191116.png"/>

<h3>Al colocar un ; en el parámetro de Foto de perfil nos revela que el servidor ejecuta curl sin sanitizarlo</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 191936.png"/>

<h3>al inyectar comandos mediante ; se obtiene una ejecución de comandos</h3>
<h3>Ejecutamos un ls y vemos los archivos internos del servidor</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 192237.png"/>

<h3>Mandamos una reverse shell a nuestra maquina atacante</h3>
<h3>Nos ponemos en escucha por netcat nc -nlvp [PUERTO]</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 192355.png"/>

<h3>Reverse shell completa</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 192410.png"/>
<h3>Hacemos el tratamiento de la TTY</h3>
<li>script /dev/null -c bash</li>
<li>crtl +z</li>
<li>stty raw -echo; fg</li>
<li>reset xterm</li>
<li>export TERM=xterm</li>
<li>export SHELL=bash</li>

<h3>Nos movemos al directorio /opt y encontramos un script backup.py en el cual tenemos permisos de escritura ya que pertenemos al grupo humanresources</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 192915.png"/>

<h3>Visualizamos el script backup.py que se ejecuta cada 60 segundos: cada minuto, elimina el backup anterior y crea uno nuevo de /opt/workconnect, ejecutándose con privilegios de root.</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 193007.png"/>

<h3>Nos aprovechamos del script que se ejecuta con permisos sudo y agregaremos lo siguiente: </h3>
<h3>echo "os.system('chmod u+s /bin/bash')" >> backup.py</h3>
<img width="956" height="776" src="../WorkConnect/image/Captura de pantalla 2026-08-10 193357.png"/>

<h3>ROOT OBTENIDO :)</h3>

<h2>Creditos a vareCruzz</h2>




