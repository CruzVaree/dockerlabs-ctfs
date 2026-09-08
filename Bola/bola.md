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

<h3>Posteriormente con el diccionario de usuarios que nos proporciono el script hacemos un ataque de fuerza bruta al servicio SSH. Vamos a poner el mismo diccionario tanto en el parametro de usuario y contraseña</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 193834.png" />

<h3>Credenciales Encontradas</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194018.png" />

<h3>Entramos por SSH</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194050.png" />

<h3>Listamos archivos ocultos y encontramos un .bash_history dentro de ahi se nota unas credenciales pertenecientes a mysql</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194325.png" />

<h3>Entramos por mysql con las credenciales respectivas y analizamos las bases de datos existentes</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194520.png" />

<h3>Encontramos la base de datos "secretito" a la cual entraremos y obtendremos la informacion que almacena</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194530.png" />

<h3>Copiamos un hash y lo analizamos con hash-identifier para descubrir que tipo de hash es</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194707.png" />

<h3>Es un hash MD5. Con los hash que almaceno la base de datos vamos a usar john para descifrar los hash</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 194919.png" />

<h3>Con el hash descifrado nos logeamos con: su baluadmin : estrella</h3>
<h3>Analizamos permisos sudo y encontramos el binario /usr/bin/unzip</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 195127.png" />

<h3>Si recordamos el contenido de .bash_history habíamos visto un archivo zip llamado "secretitosecretazo.zip", lo buscamos y vemos que se encuentra en la raiz</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 195334.png" />

<h3>Aprovechando que tenemos el binario /usr/bin/unzip descomprimos el archivo secretitosecretazo.zip y dentro se encuentra la contraseña de root</h3>
<img width="1336" height="879" src="/Bola/image/Captura de pantalla 2026-09-06 195616.png" />

<h3>ROOT OBTENIDO</h3>

<h1>Creditos a vareCruzz</h1>
<h1>Mi Lizette te amo muchisimoooooo. Viva mi novia <3</h1>











