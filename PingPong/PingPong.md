<h1>Laboratorio PingPong</h1>
<h2>Vulnerabilidades
<li>Ejecucion de comandos dentro de un servicio web.</li>
</h2>
<h2>Dificultad: Media.</h2>
<br><br>

<h2>Despleguemos el laboratorio "PingPong"</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 131724.png"/>

<h2>Mediante la herramienta nmap haremos un reconocimiento de puertos y servicios</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 131839.png"/>

<h2>
<li>Puerto 80 abierto: http</li>
<li>Puerto 442 abierto: ssl/http</li>
<li>Puerto 5000: http</li>
</h2>


<h2>Entramos a los sitios web mediante el navegador con su respectiva dirección ip y puertos</h2>
<h2>Puerto 80, no encontramos nada interesante, solo una plantilla de apache</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 131926.png"/>

<h2>Puerto 443</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 131952.png"/>

<h2>En el puerto 5000 encontramos un sitio web que realiza ping a ciertos dominios o ip</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132012.png"/>

<h2>Hacemos un ping a por ejemplo: gooogle.com</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132026.png"/>

<h2>Si ponemos atencion dentro del sitio web se ejecuta el comando ping, puede a ver la posibilidad de que podamos ejecutar otros comandos.</h2>
<h2>Cuando ponemos ";" y seguido del comando, se ejecuta ese respectivo comando en este caso whoami</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132050.png"/>

<h2>Nos ponemos en escucha por netcat: nc -nlvp (PORT)</h2>
<h2>Ahora ganaremos una reverse shell.</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132131.png"/>

<h2>EJECUTAMOS Y REVERSE SHELL COMPLETADA :)</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132145.png"/>

<h2>Hacemos tratamiento de la TTY</h2>
<h2>
<li>script /dev/null -c bash</li>
<li>crtl + z </li>
<li>stty raw -echo;fg</li>
<li>export SHELL=bash</li>
<li>export TERM=xterm</li>  
</h2>

<h2>Analizamos permisos sudo -l, y encontramos el binario /usr/bin/dkpg, mediante este binario podemos hacer un pivoting al usuario bobby. Se hara de la siguiente forma: </h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132430.png"/>
<h2>Despues: </h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 132509.png"/>

<h2>Ya que somos el usuario bobby volvemos a analizar permisos sudo -l, mediante este binario haremos un pivoting de usuario hacia el usuario: gladys. De la siguiente forma: </h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 133817.png"/>

<h2>Ya que somos el usuario gladys, analizamos permisos sudo -l y encontramos el binario /usr/bin/cut, mediante este binario podemos leer una contraseña que se encontró en el directorio /opt que pertenece a la contraseña del usuario: chocolatitio</h2>
<h2>Ejecutamos lo siguiente: (PERDON POR LA MALA CAPTURA LA TERMINAL SE HIZO CAGADA CON CADA PIVOTING)</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 142025.png"/>

<h2>chocolatito: chocolatitopassword</h2>
<h2>Ahora con la contraseña nos convertimos en chocolatito</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 142055.png"/>

<h2>Ahora analizamos los permisos sudo -l y encontramos el binario /usr/bin/awk, mediante este binario haremos otro pivoting hacia el usuario theboss</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 142251.png"/>

<h2>Posteriormente siendo el usuario theboss, analizamos permisos sudo -l y encontramos que root tiene el binario /usr/bin/sed, el cual nos permitirá escalar para ser el usuario root.</h2>
<h2>Ejecutamos lo siguiente:</h2>
<img width="956" height="776" src="../PingPong/image/Captura de pantalla 2026-09-22 142744.png"/>

<h2>ROOT OBTENIDO :)</h2>
<br>
<h1>Creditos a: vareCruzz</h1>
<h1>Mi novia me hizo un privilege escalation: empezó como novia y terminó siendo dueña de mi(MI NOVIA LIZETTE).</h1>
