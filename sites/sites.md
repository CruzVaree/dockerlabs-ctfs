<h1>Laboratorio Sites</h1>
<h1>Dificultad: Medio</h1>
<h1>Vulnerabilidades:<br> 
Explotación de un LFI <br>
RCE<br>
</h1>
<br><br>

<h2>Desplegamos el laboratorio Sites</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 195335.png"/>

<h2>Realizamos un escaneo con nmap para conocer puertos y servicios activos</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 195619.png"/>

<h2>
<li>Puerto 22 abierto: SSH</li>
<li>Puerto 80 abierto: HTTP</li>
</h2>

<h2>Entramos al sitio web y encontramos esto</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 195707.png"/>

<h2>Como no se encuentra nada interésante, vamos a realizar un fuzzing de directorios</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 200907.png"/>

<h2>Encontramos el directorio /vulnerable.php, probablemente aqui exista un LFI ya que termina en un .php, asi que buscamos el parametro vulnerable</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 200519.png"/>

<h2>Como se mencionaba aqui existe un LFI, asi que vamos a visualizarlo en el navegador</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 200613.png"/>

<h2>Ya que existe un LFI podemos aprovecharnos de ejecutar comandos mediante la herramienta: <br>
https://github.com/synacktiv/php_filter_chain_generator/blob/main/php_filter_chain_generator.py
</h2>

<h2>Generamos un "whoami"</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 201143.png"/>

<h2>Pegamos en la url http://172.17.0.2/vulnerable.php?page=php://.....</h2>
<h2>PEGAR SOLO LO GENERADO DESDE php:// hasta el final</h2>
<h2>Y como se visualiza se logro ejecutar el comando</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 201209.png"/>

<h2>Si volvemos al inicio, se mencionaban unas rutas en especial, así que vamos a ver su contenido aprovechando el LFI</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 202907.png"/>

<h2>Visualizamos el: sitio.conf</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 202921.png"/>

<h2>Se menciona un "archivitotraviesito" así que vamos a ver su contenido</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 202955.png"/>

<h2>Se encuentran las credenciales por acceso SSH (el usuario se encuentro en el archivo /etc/passwd)</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 203102.png"/>

<h2>Analizamos permisos sudo y se encuentra el binario /usr/bin/sed, el cual es vulnerable a una escalada de privilegios</h2>
<img width="956" height="776" src="/sites/image/Captura de pantalla 2026-09-26 203310.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h1>Creditos a: vareCruzz</h1>
<h1>Mi novia Lizette esta en mi corazón, en mi mente y en todo momento <3. La amo muchisimo, me motiva a mejorar en todo <3</h1>


