<h1>LABORATORIO Seeker</h1>

<h2>Despleguamos el laboratorio.</h2>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 131246.png"/>

<h3>Empezamos con el reconocimiento a través de nmap</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 131257.png"/>
<li>Puerto 80 abierto</li>

<h3>Entramos al servicio web y si leemos un poco la plantilla de apache vemos un /var/html/5eEk3r/index.html</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 131652.png"/>

<h3>Al parecer el 5eEk3r.dl pertecene a algun recurso web del sitio. Modificaremos nuestro archivo nano /etc/hosts y agregaremos el recurso</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 131750.png"/>

<h3>Realizaremos un fuzzing de subdominios a la url: http://5eEk3r.dl/</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 134758.png"/>
<h3>Encontramos un subdominio "crosswords"</h3>
<h3>De la misma forma lo agregaremos a nano /etc/hosts</h3>
<h3>Entramos al sitio web con el subdominio pero no encontraremos nada interesante</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 135121.png"/>

<h3>Por la cual realizaremos otro fuzzing de subdominios para ver que encontramos</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 135640.png"/>
<h3>Encontramos el subdomionio: admin</h3>
<h3>Al cual agregaremos de la misma forma en nano /etc/hosts</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 140429.png"/>

<h3>Entramos y vemos un panel de admin donde podemos subir archivos</h3>
<h3>Nos aprovecharemos de esto y subiremos una reverse shell hecha en php</h3>
<h3>cambiando la extesion de .php a .phar para evadir el filtro de archivos</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 140100.png"/>

<h3>Para recibir la reverse shell entramos a la ruta: </h3>
<h3>http://crosswords.5eEk3r.dl/[NombreDeTuArchivo.phar]</h3>

<h3>Nos ponemos en escucha por NETCAT nc -nlvp [PUERTO]</h3>
<h3>Reverse shell completada y analizamos los permisos sudo -l</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 140929.png"/>

<h3>Reconocemos un usuario llamado "astu" para la cual conseguiremos una consola como ese usuario</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 141028.png"/>

<h3>Si vamos al directorio /home del usuario astu, encontramos la carpeta secure, y dentro un binario con permisos SUID (permite ejecutar el binario con los permisos del propietario)</h3>

<h3>Encontramos una herramienta llamada bs64 la cual codifica mensajes a base 64</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 141328.png"/>

<h3>Nos mandamos la herramienta a nuestra maquina atacante para analizarla mejor</h3>
<h3>Desde la maquina victima levanteremos un servidor en python para obtener esa herramienta</h3>
<h3>python3 -m http.server [PUERTO]</h3>

<h3>Una vez obtenida probamos si es vulnerable a BufferOverFlow para determinar esto en crearemos 400 caracteres para convertirlos a base64 y aplicar el desbordamiento del buffer</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 144039.png"/>
<h3>Ejecutamos esos 400 caracteres dentro de la herramienta</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 144300.png"/>
<h3>Escribimos ret y nos arrogara el siguiente valor: 0x6341356341346341</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 144706.png"/>


<h3>Procedemos a casar el valor de offset en dicho valor: Tenemos que el valor del offset es el 72</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 144652.png"/>

<h3>Procedemos a usar un script para explotar este binario bs64 y obtener root</h3>
<h3>Crearemos el script dentro de la maquina victima dentro del directorio /tmp</h3>
<h3>Damos permiso de ejecucion, ejecutamos y root obtenido :)</h3>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 145706.png"/>
<img width="956" height="776" src="../Seeker/images/Captura de pantalla 2026-08-03 150309.png"/>

<h1>EXPLICACION DE LA ESCALADA</h1>
<h2>Lo que realmente se aprovecha en este ataque es un buffer overflow. El programa tiene un espacio reservado para guardar la entrada del usuario, pero no verifica si la cantidad de datos que recibe es mayor que ese espacio. Al enviar más datos de los permitidos, la información comienza a sobrescribir otras partes de la memoria, incluyendo la dirección de retorno, que es la dirección a la que el programa debe regresar cuando termina una función.

En un primer intento se envía un patrón largo para comprobar si es posible controlar esa dirección. El hecho de que el programa termine con un Segmentation Fault demuestra que la dirección de retorno fue sobrescrita con datos del usuario. Es decir, ya no contiene una dirección válida, sino parte del texto enviado.

Una vez comprobado que es posible controlar esa dirección, en lugar de escribir datos aleatorios se coloca la dirección de una función existente dentro del programa, en este caso fire(). Cuando la función actual termina y ejecuta la instrucción ret, el procesador toma esa nueva dirección y, en vez de regresar al flujo normal del programa, salta directamente a fire().

Esto funciona porque el binario no tiene PIE, por lo que la dirección de fire() siempre es la misma y puede escribirse directamente en el payload. Además, aunque NX está habilitado, no representa un problema, ya que no se está ejecutando código nuevo desde la pila, sino reutilizando una función que ya forma parte del propio programa.

En resumen, el ataque no aprovecha el Segmentation Fault; ese error solo sirve como evidencia de que fue posible sobrescribir la dirección de retorno. Lo que realmente se explota es la falta de validación del tamaño de la entrada, que permite modificar el flujo de ejecución del programa y hacer que este ejecute una función elegida por el atacante.
</h2>


<h1>Creditos a: vareCruzz</h1>



