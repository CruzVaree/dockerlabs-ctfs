<h1>Laboratorio BigWear</h1>

<h2>Despleguamos el laboratorio BigWear</h2>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-11 181151.png"/>

<h3>Realizamos un escaneo con nmap como parte del reconocimiento</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-11 181623.png"/>
<li>Puerto 80</li>
<li>Puerto 3000</li>
<li>Puerro 8000</li>

<h3>En este caso el puerto que nos interesa es el 80</h3>
<h3>Seguiremos con el reconocimiento mediante la herramienta whatweb</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-11 181713.png"/>

<h3>Vemos que mediante el puerto 80 corre un sitio web creado en wordpress</h3>
<h3>Entramos al sitio</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 182206.png"/>

<h3>Como este sitio web esta hecho en wordpress usaremos una herramienta para analizar plugins: wpscan</h3>
<h3>wpscan --url http://172.17.0.2 -e p</h3>
<h3>Encontramos un plugin vulnerable</h3>

<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 181529.png"/>

<h3>Investigando el plugin vulnerable vemos que nos permite hacer un bypass y ser administradores mediante las cookies de sesión</h3>
<h3>Usaremos esta herramienta hecha en github: https://github.com/0xgh057r3c0n/CVE-2025-34077</h3>

<h3>Obtenemos la herramienta y ejecutamos.</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 182147.png"/>

<h3>Pegamos las 2 cookies de sesión en nuestro navegador </h3>
<h3>Inspeccionar > Storage > cookies</h3>
<h3>Somos administrador</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 182329.png"/>

<h3>Entramos al perfil de administrador > Tools > Theme Editor > Theme Functions</h3>
<h3>Ejecutaremos una reverse shell en este caso usaremos esta: https://github.com/pentestmonkey/php-reverse-shell</h3>
<h3>O podemos generar esta reverse shell en php desde aqui: https://www.revshells.com/</h3>
<h3>Una vez pegada antes de guardar los cambios nos ponemos en escucha por netcat: nc -nlvp [PORT]</h3>
<h3>Guardamos cambios</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 183255.png"/>

<h3>Reverse shell completada</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 183343.png"/>

<h3>Hacemos el tratamiento de la TTY</h3>
<li>script /dev/null -c bash</li>
<li>crtl + z</li>
<li>stty raw -echo;fg</li>
<li>reset xterm</li>
<li>export SHELL=bash</li>
<li>export TERM=xterm</li>

<h3>Vemos el contenido del directorio /opt y encontramos unos scrips</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 183919.png"/>

<h3>Dentro del script start-services.sh encontramos unas credenciales</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 183929.png"/>

<h3>Posteriormente haremos un: su root</h3>
<h3>password: BigWear2024!@#</h3>
<img width="1336" height="879" src="/BigWear/image/Captura de pantalla 2026-08-14 184002.png"/>

<h3>ROOT OBTENIDO :)</h3>

<h1>CREDITOS A vareCruzz</h1>












