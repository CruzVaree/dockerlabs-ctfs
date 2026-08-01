<h1>LABORATORIO "thedog"</h1>

<h2>Despleguemos el laboratorio</h2>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 124810.png"/>

<h3>Realizamos un escaneo de nmap para conocer puertos abiertos y servicios activos dentro del laboratorio</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 125148.png"/>

<h3>Encontramos un servicio web dentro del laboratorio. Accederemos a el mediante el navegador</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 125508.png"/>

<h3>No encontramos nada interesante dentro del sitio web. Por el mismo motivo realizaremos un fuzzing de rutas/directorios</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 130307.png"/>

<h3>Se descubre una ruta html.html a la cual accederemos</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 131233.png"/>

<h3>No encontramos tampoco nada de utilidad para vulnerar.</h3>
<h3>Vamos a usar la herramienta whatweb para conocer versiones y seguir con el reconocimiento.</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 131946.png"/>

<h3>ENCONTRAMOS UNA VERSION DE APACHE ANTIGUA: 2.4.49</h3>
<h3>Investigando vulnerabilidades y CVE encontramos esto: </h3>
<h3>CVE-2021-41773</h3>
<li>Path travesal: Trata cuando un atacante puede hacer un recorrido de directorios que permite leer archivos privados dentro del servidor</li>
<li>RCE: Un atacante puedo hacer ejecucion remota de comandos</li>

<h3>Investigando el CVE se encontro la siguiente herramienta que nos permite ejecutar un RCE, la cual usaremos.</h3>
<h3>https://github.com/thehackersbrain/CVE-2021-41773</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 134250.png"/>

<h3>Ejecutamos la herramienta y nos mandamos una reverse shell</h3>
<h3>Nos ponemos en escucha por netcat nv -nlvp [PORT]</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 134431.png"/>

<h3>Reverse shell obtenida</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 134501.png"/>

<h3>Explorando que usuario existen mediante cat /ect/passwd se encuentra: "punky" </h3>
<h3>Ahora realizaremos un ataque de fuerza bruta contra la contraseña de punky</h3>
<h3>Para la cual nos pasaremos la siguiente herramienta y diccionario.</h3>
<h3>https://github.com/nohh022/bruteForce</h3>

<h3>Para pasar esta herramienta y el diccionario codificaremos a base64 desde nuestra maquina atacante y decodificaremos desde la maquina victima</h3>
<h3>Maquina atacante: cat 'force.sh' | base64 </h3>
<h3>Maquina atacante: head -n 1000 'rockyou.txt' | base64 </h3>
<h3>Maquina victima: echo 'codigo_base64' | base64 -d > force.sh </h3>
<h3>Maquina victima: echo 'codigo_base64' | base64 -d > rockyou.txt</h3>

<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 135141.png"/>

<h3>Una vez pasada la herramienta la ejecutamos contra el usuario: punky</h3>

<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 135947.png"/>

<h3>Credenciales encontradas.</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 140516.png"/>

<h3>Haremos lo mismo con el usuario "root"</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 141128.png"/>

<h3>Credenciales encontradas.</h3>
<img width="956" height="776" src="../thedog/images/Captura de pantalla 2026-08-01 141146.png"/>

<h3>ROOT OBTENIDO :)</h3>

<h2>CREDITOS A: vareCruzz</h2>
