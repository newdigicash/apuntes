# Comandos de Linux
Resumen de comandos más usados en Linux

Más info sobre los comandos:
- [Tutorial para NC][cmdNc] 
- Ejemplos de [uso de Kill][tutoKill]
- Ejemplos de [uso de lsof][tutoLsof]
- Ejemplos de [uso de netstat][tutoNetstat]

Hay una [consola para explicar los comandos][cmdShell] y sus parámetros [aquí][cmdShell].
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.2

## 2. Observación

Este archivo está sujeto a cambios

## 3. Contenido 
### 3.1 Comandos
Cmd | Descripción
:-- | :--
`ls -la ruta` | muestra todos los archivos y permisos
`netstat -ntlp` \| `grep LISTEN` | para ver los puertos abiertos 
`nc -vz host puerto` | escanea los puertos
`kill -9 PID` | mata un proceso por id
`lsof -i :puerto` | lista archivos asociados al puerto
`rm -rf carpeta` | elimina la carpeta y contenido
`ln -s origen destino` | Crea enlace simbólico
`rm enlace-simbolico` | [borra el enlace simbólico][tutoRemLn]

### 3.2 Ejemplos de uso
Escaneo de puertos 
~~~
nc -vz 192.168.0.1 80
nc -vz www.dominio.org 443
~~~

Muestra los archuivos asociados al puerto 80
~~~
sudo lsof -i TCP:80
~~~

## 4. Fuentes
Comandos básicos <https://maker.pro/linux/tutorial/basic-linux-commands-for-beginners>

Más comandos <https://www.howtogeek.com/412055/37-important-linux-commands-you-should-know/>

Consola para explicar cmd <https://explainshell.com/>

[//]: # (referencias citadas)
[cmdNc]: https://www.computerhope.com/unix/nc.htm
[cmdShell]: https://explainshell.com/
[tutoKill]: https://javarevisited.blogspot.com/2011/12/kill-command-unix-linux-example.html
[tutoLsof]: https://www.tecmint.com/10-lsof-command-examples-in-linux/
[tutoNetstat]: https://geekflare.com/netstat/
[tutoRemLn]: https://linuxize.com/post/how-to-remove-symbolic-links-in-linux
