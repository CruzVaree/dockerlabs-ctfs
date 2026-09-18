<h1>Laboratorio ApiRoot</h1>
<h2>Vulnerabilidades encontradas: fuzzing de API endpoints y manipulación de datos.</h2>
<h2>Dificultad: Media.</h2>

<br><br>
<h2>Despleguamos el laboratorio "ApiRoot"</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 165532.png"/>

<h2>Realizamos un escaneo de nmap como partel reconocimiento de puertos abiertos abiertos y servicios activos.</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 165810.png"/>

<h2><li>Puerto 22 abierto / Service SSH</li></h2>
<h2><li>Puerto 5000 Abierto / Service http</li></h2>

<h2>Entramos al sitio web mediante la direccion ip y el puerto 5000. 172.17.0.2:5000</h2>
<h2>Encontramos información sobre la API, endpoints y ejemplo de como obtener los usuarios de la API mediante un token</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 165852.png"/>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 165927.png"/>

<h2>Sabemos que la api tiene un endpoint con la ruta 172.17.0.2:5000/api/directorio_oculto/</h2>
<h2>Realizaremos un fuzzing de directorios para encontrar ese endpoint</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 170242.png"/>

<h2>El endpoint de la API es /api/users/. Asi que mandamos una peticion mediante curl.</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 171609.png"/>


<h2>Cuando realizamos la petición con curl nos da como respuesta "No autorizado". Si recordamos cuando entramos al sitio web encontramos esto: curl -H 'Authorization: Bearer password_secreta' http://172.17.0.2:5000/api/users</h2>

<h2>Así que haremos un ataque de fuerza bruta para saber la contraseña/token para acceder a los usuarios de la API</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 172212.png"/>

<h2>Una vez obtenido el token, hacemos un curl incluyendo el token</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 172307.png"/>

<h2>USUARIOS OBTENIDOS DE LA API.</h2>
<h2>Posteriormente con los usuarios de la API creamos un diccionario .txt para realizar un ataque de fuerza bruta al servicio SSH con la herramienta hydra</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 172434.png"/>

<h2>Ahora con las credenciales obtenidas entramos por SSH</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 172521.png"/>

<h2>Analizamos permisos sudo -l y encontramos el binario /usr/bin/python3 con el usuario balulero. Nos aprovechamos de ese binario para hacer un pequeño pivoting de usuario (balulero), lanzaremos una bash para obtener ese usuario.</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 172736.png"/>

<h2>Usuario balulero obtenido.</h2>
<h2>Volvemos a analizar permisos sudo -l y se encuentra el binario /usr/bin/curl</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 173114.png"/>

<h2>Para escalar privilegios manipularemos el archivo /etc/passwd y lo modificaremos sobreescribiendolo para que acceder al usuario root sin la contraseña</h2>
<h2>Visualizamos el directorio /etc/passwd y lo copiamos</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 173225.png"/>

<h2>Lo copiamos a nuestra maquina atacante y borramos la x de la primera linea de tal forma que quede asi: </h2>
<h2>root::0:0:root:/root:/bin/bash</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 173225.png"/>

<h2>Ahora desde nuestra maquina atacante levantamos un servidor en python: python3 -m http.server 80</h2>
<h2>Posteriormente desde la maquina victima con curl obtendremos ese archivo passwd modificado</h2>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 173708.png"/>

<h3>Revisamos el fichero /etc/passwd y vemos que en la primera linea no existe la x: root::0:0:root:/root:/bin/bash</h3>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 173743.png"/>

<h3>Accedemos a root</h3>
<img width="1336" height="879" src="/ApiRoot/image/Captura de pantalla 2026-09-17 173757.png"/>

<h3>ROOT OBTENIDO :)</h3>


<h1>Créditos a: vareCruzz</h1>
<h1>La persona que consiguió acceso root a mi corazón y terminó teniendo control de todo mi amor (mi novia hermosa Lizette)</h1>













