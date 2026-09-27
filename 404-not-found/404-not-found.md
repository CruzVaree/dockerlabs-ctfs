<h1>Laboratorio 404-not-found</h1>
<h1>Dificultad: Media</h1>
<h1>Vulnerabilidades: <br>
Inyeccion LDAP
</h1>
<br><br>

<h2>Despleguamos el laboratorio "404-not-found"</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 150948.png"/>

<h2>Realizamos un reconocimiento mediante nmap para conocer puertos y servicios activos</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 151214.png"/>

<h2>
<li>Puerto 22 abierto: SSH</li>
<li>Puerto 80 abierto: HTTP</li>
</h2>

<h2>Entramos al sitio web mediante el navegador y nos redirige a un hosts</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 151259.png"/>

<h2>Para acceder a ese hosts lo añadimos al fichero /etc/hosts</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 151338.png"/>

<h2>Entramos</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 151402.png"/>

<h2>Cuando hacemos click en "Participar ahora" nos manda a otra sección del sitio web</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 151739.png"/>

<h2>Encontramos dentro de esa sección un código en base64, así que lo vamos a decodificar</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 151833.png"/>

<h2>Nos da una pista de como resolver el ctf, así que realizamos un fuzzing de directorios</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 152628.png"/>

<h2>No encontramos nada.</h2>
<h2>Realizamos un fuzzing de subdominios</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 153109.png"/>

<h2>Encontramos el subdominio "info" posteriormente lo añadimos al fichero /etc/hosts</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 153146.png"/>

<h2>Encontramos un panel de login, el cual es vulnerable a una inyeccion LDAP</h2>
<h2>Inyeccion LDAP: Es un ataque informático que ocurre cuando una aplicación web no valida ni limpia correctamente los datos que ingresa un usuario, permitiendo manipular las consultas del Protocolo ligero de acceso a directorios (LDAP).</h2>
<h2>Escribimos un payload que nos devuelva true en el login, lo ponemos tanto en la contraseña como en el usuario</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 153341.png"/>

<h2>Encontramos las credenciales de Admin</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 153408.png"/>

<h2>Entramos por SSH con las dichas credenciales obtenidas</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 153522.png"/>

<h2>Analizando el contenido de la ruta /home/404-page y analizando el .bash_history, encontramos una calculadora hecha en python</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 153742.png"/>

<h2>Probamos si el script calculator.py puede interpretar comandos y ejecutarlos en vez de tratarlos solo como datos matematicos</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 154201.png"/>

<h2>Ahora somos el usuario 202-ok ya que el script interpreta comandos y lo ejecutamos como ese respectivo usuario</h2>
<h2>Analizando el directorio /home/202-ok encontramos los siguientes archivos txt</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 154411.png"/>

<h2>Vamos a probar si el contenido del archivo boss.txt la palabra: rooteable es la contraseña de root</h2>
<img width="956" height="776" src="/404-not-found/image/Captura de pantalla 2026-09-27 154438.png"/>

<h2>ROOT OBTENIDO :)</h2>

<h1>Creditos a: vareCruzz</h1>
<h1>Mi novia hermosa me motiva a mejorar en ciberseguridad. La amo mucho <3</h1>




