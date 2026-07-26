<img width="1902" height="677" alt="9" src="https://github.com/user-attachments/assets/9919017a-0530-4fbd-bccd-12ba88a8af64" /><h2>Desplegamos el laboratorio spiderport.tar</h2>

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
