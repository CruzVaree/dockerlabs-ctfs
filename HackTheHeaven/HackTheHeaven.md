<h1>Laboratorio HackTheHeaven</h1>
<h1>Dificultad: Dificil</h1>
<h1>Vulnerabilidades: Explotación de un LFI/Path Traversal hacia RCE</h1>
<br><br>
<h1>PASO IMPORTANTE ANTES DE DESPLEGAR LA MAQUINA</h1>
<h2>Dentro del auto_deploy.sh se encontro que: Internamente se está utilizando IPv6 por defecto esto provoca que internamente el uso de IPv4 quede inservible, esto afecta a cierta parte del laboratorio con respecto a la intruccion. De la siguiente forma:
<br><br>
curl http://localhost:9999
<br>
Acceso denegado.
<br><br>
curl http://127.0.0.1:9999
<br>
curl: Failed to connect to 127.0.0.1 port 9999 after 0 ms: Couldn't connect to server
</h2>
