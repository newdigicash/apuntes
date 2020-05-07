# Transferir archivos con SCP
El [comando SCP][manScp] permite transferir archivos entre servidores y dispositivos.

Aquí hay un [tutorial para cargar y descargar archivos][tutoScp] usando una llave RSA. 
Una [alternativa][tutoSftp] para transferir archivos es [SFTP][tutoSftp].
## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación
El par de claves es generado con **ssh-keygen -t rsa** y guarda archivos *id_rsa*. 
Mientras que si las claves son generadas con [putty][urlPutty], hay que exportar con 
*export openssh key* y guardar como *id_rsa*, 
tal [como se explica aquí][tutoExportKey].

## 3. Contenido 
### 3.1 Formato de SCP
El formato es *scp opciones host1 host2*. 
El formato del host es *nombre-usuario@ip-publica:/ruta-al archivo*. 

Los servers en la nube usan claves RSA, por tanto hay que usar la clave con 
la opción *-i clave-privada* y luego ingresar la passphrase.

### 3.2 Ejemplo de uso
**Envia archivo.txt** hacia la caperta de usuario en el servidor remoto. 
La carpeta remota debe tener permisos de escritura.
~~~
scp -i id_rsa archivo.txt usuario@ip-publica:/home/usuario
~~~

**Descargar archivos** desde el server remoto hacia la pc. 
La caperta remota debe tener permisos de lectura.
~~~
scp -i id_rsa usuario@ip-publica:/home/usuario/* .
~~~

## 4. Fuentes
Doc de SCP <https://linux.die.net/man/1/scp>

Tutorial de transferencia <https://www.simplified.guide/ssh/copy-file>

[//]: # (referencias citadas)
[manScp]: https://linux.die.net/man/1/scp
[urlPutty]: https://www.putty.org/
[tutoScp]: https://www.techrepublic.com/article/how-to-use-secure-copy-with-ssh-key-authentication/
[tutoExportKey]: https://www.simplified.guide/putty/convert-ppk-to-ssh-key
[tutoSftp]: https://www.simplified.guide/ssh/copy-file
