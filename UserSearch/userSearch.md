<h1>Laboratorio UserSearch</h1>

<h2>Despleguamos el laboratorio UserSearch</h2>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 195307.png"/>

<h3>Realizamos un escaneo de nmap como parte del reconocimiento</h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 195402.png"/>

<li>Puerto 22 abierto</li>
<li>Puerto 80 abierto</li>

<h3>Entramos al sitio web con la direccion ip en el navegador y encontramos un buscador de usuarios el cual es vulnerable a SQLi mediante el siguiente payload: 
admin ' or 1=1-- -</h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 201250.png"/>

<h3>Y como vemos nos muestra los usuarios de la base de datos.</h3>
<h3>Ahora mediante esas credenciales que obtuvimos guardaremos los usuarios y contraseñas en un txt para hacer un ataque de fuerza bruta al servicio SSH usando hydra</h3>

<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 201700.png"/>

<h3>Credenciales obtenidas, posteriormente entraremos por SSH</h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 201753.png"/>

<h3>Analizamos los permisos sudo -l y encontramos que podemos ejecutar como root el binario /usr/bin/python3 en el script system_info.py con la ruta: /home/kvzlx/system_info.py</h3>

<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 202022.png"/>

<h3>Analizamos el script: system_info.py</h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 202033.png"/>

<h3>Para escalar privilegios y ser el usuario root vamos a eliminar el script: system_info.py</h3>
<h3>rm -r system_info.py</h3>

<h3>Y crearemos otro script con el MISMO NOMBRE Y EL SIGUIENTE CONTENIDO: system_info.py</h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 202521.png"/>

<h3>Ahora ejecutamos el script y después de ejecutarlo esperamos un momento y revisamos si tenemos privilegios de root</h3>
<h3>sudo -u root /usr/bin/python3 /home/kvzlx/system_info.py</h3>
<h3>ls -al /bin/bash (para analizar si ya tenemos permisos root: -rwsr-xr-x 1 root root 1265648 Apr 23  2023 /bin/bash)</h3>
<h3>bash -p</h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 202653.png"/>

<h3>ROOT OBTENIDO :) </h3>
<img width="956" height="776" src="../UserSearch/image/Captura de pantalla 2026-08-27 203113.png"/>

<h1>Creditos a: vareCruzz</h1>









