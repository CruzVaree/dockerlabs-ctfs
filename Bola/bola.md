<h1>Laboratorio Bola</h1>

<h2>Desplegamos el laboratorio Bola</h2>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 190236.png" />

<h3>Realizamos un escaneo de nmap como parte del reconocimiento para conocer puertos y servicios activos y abiertos</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 190718.png" />

<h3><li>Puerto 22</li></h3>
<h3><li>Puerto 12345</li></h3>

<h3>Entramos al sitio web mediante un navegador colocando la dirección ip: 172.17.0.2:12345</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 190754.png" />

<h3>No vemos nada interesante dentro de la pagina, por lo cual realizaremos un fuzzing de directorios</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 192246.png" />

<h3>Encontramos la ruta /login</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 192256.png" />
<h3>Encontramos la ruta /user</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 192304.png" />

<h3>Analizando un poco la ruta /user vemos que tenemos que proporcionar un usuario valido esto significa que podemos tomar un vector de ataque que puede ser enumerar los usuarios de la API. Vamos a analizar como se enumeran los usuarios dentro de la API si es con un nombre o con un id</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 192404.png" />

<h3>A lo que vemos los usuarios se enumeran mediante un id, por lo cual vamos a crear un script en python que haga esta enumeracion</h3>
<h3>Usaremos el siguiente script: (https://github.com/CruzVaree/scripts_hacking/blob/main/enumerarApi.py)</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 193501.png" />




