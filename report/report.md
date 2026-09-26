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

<h2>Es posible explotar un LFI A RCE, ya que tenemos el directorio "info.php" visible, el file_uplaods activo y un LFI <br>
Asi que usaremos el siguiente script para realizar esto: https://github.com/CruzVaree/scripts_hacking/blob/main/lfi2rce/phpinfolfi.py
</h2>

<h2>PASOS IMPORTANTES</h2>
<h2>Haremos algunos cambios en el script y serán los siguientes: </h2>
<h2>1-Añadimos en la linea 2: # -*- coding: utf-8 -*-</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 132403.png"/>

<h2>2-Cambiamos esta parte y añadimos esto: LFIREQ = """GET /about.php?file=%s HTTP/1.1\r\n
</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 131611.png"/>

<h2>3-De acuerdo a nuestra dirección ip y puerto donde queremos recibir la reverse shell cambiaremos esas variables</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 132416.png"/>

<h2>4-Cambiamos la linea 237 de la siguiente forma: host = socket.gethostbyname(sys.argv[1]) por: host = sys.argv[1]</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 133722.png"/>

<h2>Nos ponemos en escucha por netcat de acuerdo al puerto puesto en el script:<br>
nc -nlvp (PUERTO)</h2>
<h2>Ejecutamos</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 133828.png"/>

<h2>Reverse shell completada</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 133845.png"/>
<h2>Hacemos tratamiento de la TTY</h2>
<h2><li>script /dev/null -c bash</li></h2>
<h2><li>ctrl + z</li></h2>
<h2><li>stty raw -echo;fg</li></h2>
<h2><li>reset xterm</li></h2>
<h2><li>export SHELL=bash</li></h2>
<h2><li>export TERM=xterm</li></h2>

<h2>Explorando rutas encontramos el siguiente script: config.php, que contiene la conexion a una base de datos pero no nos servira de mucho</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 134605.png"/>

<h2>Explorando los directorios dentro de /var/www/html/desarrollo vemos que tenemos git instalado y podremos sacar informacion sensible viendo que hay dentro de</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 135621.png"/>

<h2>Y como vemos no nos permite ver, nos da un error: To add an exception for this directory, call:</h2>

<h2>Ya que si intentamos ingresar como directorio seguro no nos va a dejar por lo que haremos lo siguiente:</h2>
<h2>
export HOME=/tmp<br>
git config --global --add safe.directory /var/www/html/desarrollo/.git</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 135904.png"/>

<h2>Ahora si nos dejara ver los log de .git</h2>
<h2>Analizando los commit: me interesa mas el del usuario adm</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 135955.png"/>

<h2>Investigamos ese commit</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 140107.png"/>

<h2>Y encontramos la contraseña de adm:9fR8pLt@Q2uX7dM^sW3zE5bK8nQ@7pX</h2>
<h2>Usamos la credenciales para ingresar como ese usuario</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 140208.png"/>

<h2>Dentro del directorio /home/adm encontramos un archivo .bashrc</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 140327.png"/>

<h2>con cat .bashrc vemos el contenido y encontramos una contraseña en hexadecimal</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 140444.png"/>

<h2>Pasamos la contraseña en hexadecimal a texto plano</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 140705.png"/>

<h2>Probaremos dicha contraseña: dockerlabs4u con el usuario root y veremos que somos root.</h2>
<img width="956" height="776" src="/report/image/Captura de pantalla 2026-09-26 140731.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h1>Créditos a: vareCruzz</h1>
<h1>
Para mi novia: <br>
Mi corazón está cifrado, pero contigo la contraseña siempre funciona.
</h1>





