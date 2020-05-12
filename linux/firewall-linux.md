# Firewall en Linux
El [firewall][urlFirewall] es un conjunto de reglas que filtran el tráfico desde y hacia el server.

Aunque los devops usan o usaban [IPTables][urlTutoIptables], en Ubuntu es más fácil 
usar [UFW][urlUFW].

Hay un [tutorial][urlTutoUFW] que explica cómo usar el [comando ufw][urlTutoUFW] y sus parámetros. 
Aquí hay [otro tutorial][urlTutoUfwUbuntu] para Ubuntu.

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.1

## 2. Observación

En Debian también funciona el comando y los parámetros.

## 3. Contenido 

### 3.1 Debian / Ubuntu

Cmd | Descripción
:-- | :--
`sudo apt install ufw` | Descarga e instala UFW
`sudo ufw disable` | deshabilita el firewall
`sudo ufw allow puerto` | habilita el _puerto_
`sudo ufw allow from ip-publica to any port puerto` | Regla para _ip-publica_ y puerto específico
`sudo ufw status numbered` | muestra el estado con números
`sudo ufw delete numero` | quita la regla por _numero_
`sudo ufw allow/deny puertoIni:puertoFin/protocolo` | habilita o deshabilita rango de puertos
`sudo ufw deny from ip-publica` | bloquea acceso a _ip-publica_
`sudo ufw enable` | habilita el firewall
`sudo ufw reset` | quita la reglas

#### 3.1.1 Ejemplos de uso
Habilita SSH para una IP pública
~~~
sudo ufw allow 192.168.0.1/32 to any port 22
~~~

Deshabilita el puerto 80
~~~
sudo ufw deny http
~~~

Habilita el puerto 443
~~~
sudo ufw allow https
~~~

Bloquea acceso a MySQL
~~~
sudo ufw deny 3306/tcp
~~~

Habilita el rango de puertos

~~~
sudo ufw allow 8000:8005/tcp
~~~

## 4. Fuentes
Firewall Wiki <https://en.wikipedia.org/wiki/Firewall_(computing)>

UFW Wiki <https://en.wikipedia.org/wiki/Uncomplicated_Firewall>

Tutorial de UFW <https://www.hostinger.es/tutoriales/como-configurar-firewall-ubuntu>

[//]: # (referencias citadas)
[urlTutoIptables]: https://www.redeszone.net/gnu-linux/iptables-configuracion-del-firewall-en-linux-con-iptables
[urlUFW]: https://en.wikipedia.org/wiki/Uncomplicated_Firewall
[urlFirewall]: https://en.wikipedia.org/wiki/Firewall_(computing)
[urlTutoUFW]: https://www.hostinger.es/tutoriales/como-configurar-firewall-ubuntu
[urlTutoUfwUbuntu]: https://www.digitalocean.com/community/tutorials/como-configurar-un-firewall-con-ufw-en-ubuntu-18-04-es
