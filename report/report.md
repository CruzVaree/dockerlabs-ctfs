<h1>Laboratorio Report</h1>
<h1>Dificultad: Medio</h1>
<h1>Vulnerabilidades:<br> 
Explotación de un LFI/Path Traversal hacia RCE <br>
Bypass de subida de ficheros mediante el Content-Type <br>
Inyección SQL</h1>
<br><br>

<h2>Despleguamos el laboratorio Report</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 125322.png"/>

<h2>Realizamos un escaneo de nmap</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 125422.png"/>

<h2>
<li>Puerto 22 abierto: ssh</li>
<li>Puerto 80 abierto: http</li>
<li>Puerto 3306 abierto: mysql</li>
</h2>

<h2>Al entrar al sitio web mediante la dirección ip: 172.17.0.2 nos redirige al siguiente hosts: realgob.dl <br>
Agregamos este hosts al /etc/hosts</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 125540.png"/>

<h2>Entramos al realgob.dl y encontramos lo siguiente</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 125605.png"/>

<h2>Hacemos un fuzzing de directorios para seguir con el reconocimiento</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 130105.png"/>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 130348.png"/>

<h2>Especialmente mas que nada me llama la atención el directorio /info.php y varias rutas que tienen el .php ya que aqui se puede acontener un LFI</h2>
<h2>info.php: es visible y tiene activo el file_uploads esto nos permite subir archivos</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 130535.png"/>


<h2>Despues de analizar posibles rutas donde pueda existir un LFI encuentro lo siguiente</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 131210.png"/>

<h2>El parametro file me permite ver el fichero /etc/passwd, aqui tenemos un LFI dentro de la ruta /about.php</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 132459.png"/>

<h2>Es posible realizar un explotar un LFI A RCE, ya que tenemos el directorio "info.php" visible, el file_uplaods activo y un LFI <br>
Asi que usaremos el siguiente script para realizar esto: https://github.com/CruzVaree/scripts_hacking/blob/main/lfi2rce/phpinfolfi.py
</h2>



