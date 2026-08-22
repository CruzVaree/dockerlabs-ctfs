<h1>Laboratorio Inclusion</h1>
<h2>Desplegamos el laboratorio</h2>

<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 192355.png" />

<h3>Como parte del reconocimiento haremos un escaneo de nmap</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 193052.png" />


<h3>Encontramos un sitio web activo dentro de la maquina. Entraremos al sitio web mediante el navegador y direccion ip</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 193122.png" />

<h3>No encontramos nada interesante por lo cual realizaremos un fuzzing de directorios</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 193318.png" />

<h3>Encontramos un directorito llamado /shop/ al cual nos dirigiremos</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 193341.png" />

<h3>Vamos a realizar otor fuzzing de directorios</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 193620.png" />

<h3>Entramos al index.php y abajo aparece algo interesante que es:"Error de sistema: ($_GET('archivo')");</h3>
<h3>Mediante esa palabra "archivo" nos puede indicar un parametro para hacer un Local File Inclusion</h3>
<h3>Vamos a probarlo</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-17 193817.png" />

<h3>Confirmado mediante "archivo" podemos hacer un LFI, dentro de /etc/passwd encontramos dos usuarios</h3>
<li>seller</li>
<li>manchi</li>
<h3>Mediante esos dos usuarios intentaremos hacer un ataque de fuerza bruta con hydra al servicio SSH para obtener la contraseña</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-21 182038.png" />

<h3>Credenciales obtenidas.</h3>
<h3>Entramos mediante SSH con el usuario manchi y contraseña: lovely</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-21 182113.png" />

<h3>Poniendo a analizar no tenemos una forma como tal elevar privilegios, entonces cambiaremos de usuario a seller</h3>
<h3>Para ello vamos a intentar descifrar su contraseña</h3>
<h3>Desde nuestra maquina atacante nos pasamos el rockyou.txt y la siguiente herramienta a la maquina victima</h3>
<h3>Herramienta: https://github.com/nohh022/bruteForce</h3>

<h3>Desde la maquina victima nos vemos al directorio /tmp y desde la maquina atacante pasamos la herramienta y el rockyou</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-21 182750.png" />
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-21 182756.png" />

<h3>Damos permisos de ejecución a la herramienta y ejecutamos.</h3>
<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-21 183143.png" />

<h3>Ya que somos el usuario seller analizamos los permisos sudo -l y encontramos el binario /usr/bin/php el cual explotaremos y seremos root de la siguiente forma: </h3>

<img width="1336" height="879" src="/inclusion/image/Captura de pantalla 2026-08-21 183353.png" />

<h3>ROOT OBTENIDO :) </h3>

<h1>Creditos a: vareCruzz</h1>















