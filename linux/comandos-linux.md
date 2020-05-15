# Comandos de Linux
Resumen de comandos más usados en Linux

Más info sobre los comandos:
- [Tutorial para NC][cmdNc] 
- Ejemplos de [uso de Kill][tutoKill]
- Ejemplos de [uso de lsof][tutoLsof]
- Ejemplos de [uso de netstat][tutoNetstat]
- Ejemplos de [uso de chown][tutoChown]
- Ejemplos de [uso de mkdir][tutoMkdir]

Hay una [consola para explicar los comandos][cmdShell] y sus parámetros [aquí][cmdShell].
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.4

## 2. Observación

Este archivo está sujeto a cambios

## 3. Contenido 
### 3.1 Comandos
Cmd | Descripción
:-- | :--
`ssh -i llave-privada usuario@host` | conecta al server remoto vía ssh
`scp -r host:ruta/carpeta .` | [Descarga la carpeta remota][tutoScp] a la carpeta actual
`ls -la ruta` | muestra todos los archivos y permisos
`netstat -ntlp` \| `grep LISTEN` | para ver los puertos abiertos 
`nc -vz host puerto` | [escanea los puertos][tutoOpenPorts]
`nmap -sT -pinicio-fin host` | [escanea los puertos][tutoOpenPorts] *inicio* a *fin*
`kill -9 PID` | mata un proceso por id
`lsof -i :puerto` | lista archivos asociados al puerto
`rm -rf carpeta` | elimina la carpeta y contenido
`ln -s origen destino` | Crea enlace simbólico
`rm enlace-simbolico` | [borra el enlace simbólico][tutoRemLn]
`mkdir -p directorio/subdirectorio` | Crea carpeta y subcarpeta
`chown -R usuario directorio` | Cambia de propietario a caperta y contenido

### 3.2 Ejemplos de uso
Escanea un solo puerto con NC y Nmap. 
~~~
nc -vz www.dominio.org 443
nmap -sT -p80 dominio.com
~~~

Escanea un rango de puertos con NC y Nmap.
~~~
nc -vz 192.168.0.1 80-90
nmap -sT -p80-90 192.168.0.1
~~~

Muestra los archivos asociados al puerto 80
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
[tutoMkdir]: https://ayudalinux.com/como-usar-el-comando-mkdir
[tutoChown]: https://www.servidoresadmin.com/comando-chown-en-linux
[tutoScp]: https://www.ssh.com/ssh/scp
[tutoOpenPorts]: https://linuxize.com/post/check-open-ports-linux
