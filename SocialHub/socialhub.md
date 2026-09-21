<h1>Laboratorio SocialHub</h1>
<br>
<h2>Vulnerabilidades
<li>XSS reflejado: Es una vulnerabilidad de seguridad informática que permite a un atacante inyectar código malicioso (normalmente JavaScript) en una página web legítima para que se ejecute en el navegador de otros.</li>
</h2>
<h2>Dificultad: Dificil.</h2>
<br><br>

<h2>Despleguemos el laboratorio "SocialHub"</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 192808.png"/>

<h2>Lanzamos como parte de reconocimiento un escaneo de servicios y puertos con NMAP</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 192844.png"/>
<h2>
<li>Puerto 22 abierto: Service SSH</li>
<li>Puerto 5000 abierto: Service http</li>
</h2>

<h2>Entramos a la web: 172.17.0.2:5000</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193043.png"/>

<h2>Dentro del sitio web encontramos una red social, a la cual vamos a registrarnos e iniciar sesion.</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193107.png"/>

<h2>Una vez iniciando sesión encontramos el objetivo del laboratorio que es básicamente explotar un XSS reflejado a través de una imagen SVG</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193142.png"/>

<h2>Encontramos la función donde podemos cambiar nuestra foto de perfil mediante la opción de subir una imagen y se comenta que esa parte se acontece que los archivos SVG se suben sin validar su contenido</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193217.png"/>

<h2>Nos vamos a crear un archivo SVG con el siguiente contenido (este codigo solo nos mostrara una alerta en el navegador): </h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193421.png"/>

<h2>subimos nuestro XSS.svg a la función de cambiar perfil, actualizamos la pagina y vemos que se ejecuta el XSS</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193504.png"/>

<h2>Se realizara un ataque XSS reflejado que tendra como proposito robar las cookies de sesion de los usuarios (admin), nos aprovecharemos del HttpOnly que tiene como valor "false", esto nos permite que el valor false podamos acceder a las cookies de sesion debido a que: permite que los scripts de JavaScript accedan a ella a través de la propiedad document.cookie</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193621.png"/>
<h2>Para realizar ese ataque creamos el siguiente archivo SVG: </h2>
<h2>PUNTO A TOMAR EN CUENTA: DEBEN AJUSTAR EL ARCHIVO DE ACUERDO A SU DIRECCION IP Y EL PUERTO EN LA QUE QUIERAN RECIBIR LAS COOKIES.</h2>
<h2>PONER LA DIRECCIÓN IP DE LA INTERNAZ DE KALI. NO PONER LA DIRECCION IP DE DOCKERLABS</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 193900.png"/>

<h2>1-Nos levantamos un servidor en python de acuerdo al puerto puesto en el archivo SVG: python3 -m http.server (PORT)</h2>
<h2>2-Subimos el archivo SVG a nuestra foto de perfil.</h2>
<h2>3-Esperamos que lleguen las cookies de sesion</h2>
<h2>Hemos obtenido la cookie de sesión de admin.</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 194146.png"/>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 194216.png"/>

<h2>Una vez obtenida la cookie la pegamos en el navegador: Inspeccionar > Storage > Cookies</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 194303.png"/>

<h2>Entramos por SSH</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 194418.png"/>

<h2>Analizamos binarios para escalar privilegios y encontramos el binario /usr/bin/env, el cual es vulnerable para obtener root</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 194650.png"/>

<h2>ROOT OBTENIDO</h2>
<h2>FLAG ROOT</h2>
<img width="956" height="776" src="../SocialHub/image/Captura de pantalla 2026-09-20 194858.png"/>


<h1>Creditos a: vareCruzz</h1>
<h1>AMO MUCHO A MI NIÑA BONITA LIZETTE <3</h1>


