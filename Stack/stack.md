<h1>Laboratorio Stack</h1>

<h2>Desplegamos el laboratorio Stack</h2>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 201142.png" />

<h3>Realizamos un escaneo de nmap como parte del reconocimiento</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 201412.png" />
<li>Puerto 22</li>
<li>Puerto 80</li>

<h3>Encontramos un sitio web al cual accederemos mediante el navegador con la dirección ip</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 201443.png" />

<h3>Analizando el código fuente encontramos una posible brecha de entrada</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 201456.png" />

<h3>Vamos a realizar un fuzzing web para encontrar directorios ocultos</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 201725.png" />

<h3>Vamos a encontrar un note.txt y nos dirigiremos a ese directorio</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 201738.png" />
<h3>El funcionamiento de str_replace() sirve para sustituir ciertos caracteres por espacios en blanco. (punto importante a tomar)</h3>

<h3>También encontramos un directorio file.php donde puede que se acontezca ese LFI del que habla la note.xt </h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 203308.png" />


<h3>Vamos a realizar un fuzzing probando con diferentes parametros intentando leer el passwd</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 203015.png" />
<h3>En este caso usaremos el siguiente bypass: ....//....//....//....//etc/passwd. Ya que recordemos se "Parcho" el LFI mediante el str_replace()</h3>

<h3>El parámetro que nos dejara realizar el LFI es el "file"</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 203318.png" />
<h3>Hemos logrado ver el passwd</h3>

<h3>Volviendo a tomar lo que analizamos el código fuente, vamos a la ruta que encontramos ahi.</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 203347.png" />

<h3>Entramos por ssh mediante las credenciales encontradas</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-29 203428.png" />

<h3>Una vez dentro analizamos permisos SUID</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 121358.png" />

<h3>Analizando vemos que tenemos permisos SUID en el script /command_exec</h3>
<h3>Ejecutamos el programa para analizar su funcionamiento</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 121515.png" />

<h3>Ahora analizaremos si es posible realizar un BufferOverflow</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 121557.png" />

<h3>Y en efecto nos da un segmentation faul. Antes que todo debemos entender que es un BufferOverFlow</h3>
<h3>BufferOverflow:Es un error de un programa que ocurre cuando intenta almacenar más datos en un espacio de memoria reservado —llamado búfer— de los que este puede contener.
buffer:Espacio de almacenamiento temporal en la memoria que guarda datos mientras se trasladan de un lugar a otro</h3>

<h3>Vamos a pasarnos el programa a nuestra maquina atacante y analizaremos de una forma mejor el programa</h3>
<h3>Nos levantamos un servidor en python desde la maquina victima</h3>
<h3>maquina victima: python3 -m http.server 8080</h3>
<h3>Entramos y descargamos el programa</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 121705.png" />


<h3>Una vez pasado el script vamos a crear una cadena de caracteres para volver a desbordar</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 122741.png" />

<h3>Desde la herramienta gdb ejecutaremos el programa y pasaremos la cadena de caracteres</h3>
<h3>Haremos un run</h3>
<h3>Y después un ret</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 122820.png" />

<h3>Una vez puesto el ret encontramos el rip: 0x3164413064413963</h3>
<h3>El RIP es la dirección de memoria de la instrucción que la CPU va a ejecutar 
significa que el programa intentó continuar la ejecución en esa dirección.</h3>

<h3>Ya que tenemos el valor del RIP vamos a obtener el valor offset</h3>
<h3>offset: Representa un valor numérico (desplazamiento) que indica la posición exacta entre el inicio de un objeto o bloque de memoria y un elemento específico dentro del mismo</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 122846.png" />

<li>Valor del offset:88</li>

<h3>Ya que tenemos estos datos vamos a crear un exploit para llegar a la ejecución de comandos</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 125921.png" />

<h3>El sentido de este script es que sirve para automatizar el envío del payload a un binario vulnerable mediante la entrada estándar (stdin), con el objetivo de probar o explotar un desbordamiento de búfer (Buffer Overflow).</h3>

<h3>Ejecutamos el script</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 130058.png" />
<h3>Cuando ejecutamos el script obtendremos ciertos valores estos significa que se logro sobrescribir con éxito la variable key en memoria, pero aún no tienes el valor correcto para obtener el acceso de administrador.</h3>
<h3>La key es una variable en la memoria del programa que funciona como un seguro o bandera de control de acceso. El programa la consulta para decidir qué nivel de permisos darte</h3>

<h3>Ahora haremos lo siguiente</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 130352.png" />
<h3>QUE PASA AQUI</h3>
<h3>El valor cambió a 42424242: En código ASCII, la letra "B" equivale al número hexadecimal 42. Al ver cuatro 42 juntos, significa que la variable key ahora fue sobrescrita completamente por tus letras "B".</h3>
<h3>
Las 76 letras "A" rellenaron todo el espacio seguro del búfer en memoria sin tocar key.
Las letras "B" empezaron a escribir directamente sobre el espacio de key.
El offset correcto: Esto confirma que necesitas 76 bytes de relleno (las "A") antes de colocar la clave de acceso.
Al ejecutarlo con esos 76 caracteres de relleno seguidos de \xad\xde, la variable key tomará el valor 0xdead y se obtendra la salida de modo administrador.</h3>

<h3>Ahora ya que obtuvimos el valor de la offset modificaremos el script</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 131840.png" />

<h3>Con el script se obtuvo lo siguiente:
Se redujo la cantidad de letras "A" de 88 a 76 para llenar exactamente el bufer
dead = b"\xad\xde": Se define los bytes hexadecimales de 0xdead
payload = RIP + dead: Se unio los 76 bytes de relleno ("A") con los 2 bytes hexadecimales deseados (\xad\xde), creando una carga útil total de 78 bytes
La variable key recibirá el valor hexadecimal correcto (0xdead), lo que debería darnos el modo administrador.
</h3>
<br>
<h3>Ejecutamos el script</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 131906.png" />
<h3>Y somos modo administrador ahora modificamos el script para obtener ejecución de comandos</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 132144.png" />


<h3>Ejecutamos y en efecto podemos ejecutar comandos</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 132203.png" />

<h3>Ahora como ultimo paso vamos a modificar el comando a ejecutar y cambiamos esto</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 132407.png" />
<h3>Que modificara el /etc/passwd para ser root sin que nos pidan la contraseña</h3>

<h3>Copiamos el script a la maquina victima en el directorio tmp</h3>
<h3>Por ultimo una vez copiado el script en la maquina victima modificamos esto nadamas</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 134826.png" />

<h3>Damos permisos de ejecución y ejecutamos</h3>
<img width="1166" height="858" alt="1" src="/Stack/image/Captura de pantalla 2026-08-30 135040.png" />

<h3>ROOT OBTENIDO</h3>

<h1>Creditos a: vareCruzz</h1>






















