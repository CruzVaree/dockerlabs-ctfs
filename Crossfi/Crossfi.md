<h1>Laboratorio Crossfi</h1>
<h1>Dificultad: Media</h1>
<h1>Vulnerabilidades: CSRF (Cross-Site Request Forgery o Falsificación de Peticiones en Sitios Cruzados)</h1>
<br><br>
<h1>Despleguemos el laboratorio Crossfi</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 192251.png"/>

<h1>Realizamos un escaneo de nmap para conocer puertos y servicios activos</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 192445.png"/>

<h2>
<li>Puerto 22 abierto: SSH</li>
<li>Puerto 5000 abierto: HTTP</li>
</h2>

<h1>Entramos al sitio web mediante el navegador y encontramos un panel de inicio de sesion</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 192607.png"/>

<h1>Nos creamos una cuenta e iniciamos sesion</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 192659.png"/>

<h1>Una vez iniciado sesion si ingresamos a la seccion de panel nos encontramos un vector vulnerable a un CSRF</h1>
<h1>Encontramos que podemos cambiar la contraseña, posteriormente actualizamos nuestra contraseña </h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 193529.png"/>

<h1>Interceptamos esta peticion de cambio de contraseña con burpsuite</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 194654.png"/>

<h1>Si cambiamos la direccion ip de Origin por: 127.0.0.1 pasara que: Si el servidor acepta ambas solicitudes y cambia la contraseña, eso sugiere que podría no estar validando correctamente el encabezado Origin.</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 194728.png"/>

<h1>Mandamos la petición con la cabecera de Origin modificada nos darán unas credenciales</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 194752.png"/>

<h1>Como se nos indica en el siguiente laboratorio existe un campo que no tiene tokens protegidos contra CSRF</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 194849.png"/>

<h1>Así que con burpsuite analizamos cuales son y se encuentra que el campo "Biografía" es vulnerable</h1>
<h1>Ya que no existe ese token que protege contra CSRF</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 195110.png"/>

<h1>EJEMPLO DE COMO SE VEIAN LOS DEMÁS CAMPOS CON UN TOKEN CSRF</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 195120.png"/>

<h1>Interceptamos la petición cuando cambiamos la Biografía y modificamos de la misma manera la cabecera "Origin"</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 195259.png"/>

<h1>Mandamos la peticion y nos proporcionaran las credenciales para ingresar por SSH</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 195328.png"/>

<h1>Entramos por SSH</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 195651.png"/>

<h1>Analizamos binarios SUID y encontramos el binario /usr/bin/env, el cual es vulnerable para escalar privilegios, así que nos haremos la escalada a root mediante este SUID</h1>
<img width="1336" height="879" src="/Crossfi/image/Captura de pantalla 2026-10-24 195828.png"/>

<h1>ROOT OBTENIDO. :)</h1>

<h1>Créditos a: vareCruzz</h1>
<h1>Mi novia me hackeo el corazón y ahora es suyoo <3</h1>






