<h1>Laboratorio DataBase</h1>
<h2>Despleguamos el laboratorio database</h2>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 183755.png"/>


<h3>Realizamos un escaneo de nmap para conocer servicios y puertos activos</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 183915.png"/>
<li>Puerto 22</li>
<li>Puerto 80</li>
<li>Puerto 139</li>
<li>Puerto 445</li>
<h3>Entramos al sitio web mediante la dirección ip y encontramos este login</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 183956.png"/>

<h3>Identifique que se puede bypassear ese login mediante una SQLi usando el siguiente payload, colocándolo en el usuario y contraseña: </h3>
<h3>' OR 1 = 1 -- -</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184015.png"/>

<h3>Detectamos que el sitio web es vulnerable a SQLi para la cual usaremos la herramienta sqlmap</h3>
<h3>1. Encontraremos las bases de datos existentes</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184447.png"/>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184747.png"/>

<h3>2. Vamos a encontrar las tables de la base de datos register</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184821.png"/>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184831.png"/>

<h3>3. Encontramos la tabla users vamos a obtener sus columnas</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184905.png"/>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 184918.png"/>

<h3>4. Ahora mostramos la informacion de esas columnas</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 185008.png"/>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 185017.png"/>

<h3>Recordemos que en el escaneo de nmap encontramos un servicio de Samba smb (puerto 139), que sirve para compartir archivos, impresoras y otros recursos a diferentes equipos</h3>
<h3>Ahora con las credenciales que obtuvimos con sqlmap vamos a probarlas en el servicio Samba smb</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 185341.png"/>

<h3>Encontramos carpetas compartidas, vamos a explorar el directorio shared</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 185409.png"/>

<h3>Encontramos un .txt el cual vamos a descargar para verlo desde nuestra maquina atacante</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 191259.png"/>

<h3>Analizamos el contenido augustus.txt</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 191445.png"/>

<h3>En el contenido almacena un hash el cual mediante la herramienta hash-identifier nos ayudara a saber el tipo de hash y descifrarlo</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 191521.png"/>

<h3>Es un hash MD5, vamos a descifrarlo</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 192515.png"/>

<h3>Mediante SSH entraremos con el usuario: augustus y la contraseña: lovely</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 192605.png"/>

<h3>Una vez adentro analizamos permisos sudo -l y encontramos que dylan tiene el binario /usr/bin/java</h3>
<h3>Probaremos haciendo un: su dylan y poniendo la contraseña que encontramos en la base de datos: </h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 192843.png"/>

<h3>Efectivamente con esa contraseña se pudo ser el usuario dylan</h3>
<h3>Analizamos los permisos SUID y encontramos el binario /usr/bin/env, el cual es vulnerable para ser el usuario root</h3>
<img width="956" height="776" src="../database/image/Captura de pantalla 2026-09-03 193408.png"/>

<h3>ROOT OBTENIDO :)</h3>

<h1>Creditos a: vareCruzz y creditos a mi novia Lizette que me motiva a mejorar en todo <3</h1>














