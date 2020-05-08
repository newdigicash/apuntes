# Certificado SSL / TLS
Para obtener un [certificado SSL][urlSSLGlobal], hay que generar un CSR en nuestro server, 
luego enviar a una [entidad certificadora (también CA)][urlCAWiki]. 
Luego de la verificación, la entidad envía los certificados listos para usarse.

Los [tipos de certificados][urlTipoSSL] comunes son:
+ *Single-Domain* para un solo dominio. Incluye prefijo _www_.
+ *Wildcard* para un dominio y subdominios Ejemplo: _mail.dominio.com_
+ *Multi-domain* donde un certificado sirve para varios dominios. Costo x dominio.

Hay tres [tipos de niveles de validación][urlTipoSSL]:
+ _DV_ para validación de dominio. Pone solo candado en la barra
+ _OV_ para validación de organización. Muestra nombre de la org en la barra.
+ _EV_ para una validación extendida. Pone toda la barra verde.

La _OV_ y _EV_ require documentación para  la verificación de el(los) dominio(s), 
por lo que tarda días en emitirse. En tanto que _DV_ no, por eso se emite en minutos.

## 1. Metadatos

### Autor
@newdigicash
### Versión
0.3

## 2. Observación
[Certbot][urlCertbot] obtiene el certificado desde [Lets Encrypt][urlLetsEncrypt] y ayuda 
a renovarlo automáticamente.

## 3. Contenido 

### 3.1 Certificado para development
Para desarrollo, la CA [Lets Encrypt][urlLetsEncrypt] emite certificados gratuitos. 
Hay una [herramienta para generar certificados SSL][urlSSL4Free] a nivel _DV_.

Hay 2 formas para generar un CSR y la llave privada: 
+ con [keytool][urlKeytool] y 
+ con [openssl][urlOpenssl].

La [herramienta][urlSSL4Free] acepta un [CSR][urlCSR] propio o sino lo genera 
automáticamente y realiza la validación.

Para validar el dominio hay dos formas:

1. [Cargar 2 archivos][urlTutoScp] estáticos a la carpeta pública vía FTP o 
2. Agregar registros _TXT_ en los DNS del dominio. Funciona con _TTL_ de 300 segundos

Una vez validado, la CA permite descargar los archivos [*ca_bundle.crt*][urlCABundle] , 
*certificate.crt* que corresponde al dominio, y *private.key*.  Este último tiene información 
solamente si la CA generó el CSR por nosotros, caso contrario viene vacía.

### 3.2 Certificado para production
Hay [varias Autoridades de Certificación (CA)][urlListaCA] como:
+ [Namecheap][urlNamecheap] se basa en [Comodo Store][urlComodoStore].
+ [Comodo Store][urlComodoStore] ofrece certificados económicos.
+ [Godaddy SSL][urlGodaddy] Precios similares a [Comodo SSL][urlComodo]
+ [Comodo SSL][urlComodo] para negocios medianos. Costoso
+ [DigiCert][urlDigicert] para negocios serios. Más costoso

Cada CA tiene su proceso para la emisión de certificados. Algunas CA generan el CSR 
con la información que nosotros proporcionemos, mientras que otras CA piden que 
enviemos el archivo CSR.

Se puede generar el CSR con [keytool][urlKeytool] o con [openssl][urlOpenssl]. 
Luego enviar el CSR a la CA y seguir el proceso de validación y verificación.

Según el tipo de nivel, la CA podrá pedir documentación para validar el dominio 
y la organización. Este proceso podría tomar varios días.

Una vez completado el proceso de verificación. La CA envía el certificado al email 
o permite la descarga desde la cuenta de usuario.

#### 3.2.1 Conversión de formatos
La CA envía varios archivos como *dominio.crt*, *root.crt*, *intermediate.crt*, 
y *private.key*, o en algunos casos envian [certificado en PEM][urlPem].

Para [crear un PEM][urlPem] a partir de los CRT independientes:
~~~
sudo cat dominio.crt intermediate.crt root.crt >> dominio.pem
~~~

También se puede armar manualmente con un editor de texto, 
respetando el [orden de pem][urlPem].

En [esta web][urlConverterFormato] hay indicaciones 
[para convertir desde y hacia otros formatos][urlConverterFormato].

## 4. Fuentes
Wiki de SSL <https://es.wikipedia.org/wiki/Seguridad_de_la_capa_de_transporte>


[//]: # (referencias citadas)
[urlCSR]: https://www.dondominio.com/help/es/242/que-es-csr/
[urlCABundle]: https://www.namecheap.com/support/knowledgebase/article.aspx/986/69/what-is-ca-bundle
[urlCAWiki]: https://en.wikipedia.org/wiki/Certificate_authority
[urlSSLGlobal]: https://www.globalsign.com/es/centro-de-informacion-ssl/que-es-ssl/
[urlTipoSSL]: https://www.cloudflare.com/learning/ssl/types-of-ssl-certificates/
[urlLetsEncrypt]: https://letsencrypt.org
[urlSSL4Free]: https://www.sslforfree.com
[urlPem]: https://www.digicert.com/es/apoyo-tecnico/crear-archivo-pem.htm
[urlTutoScp]: https://github.com/newdigicash/apuntes/blob/master/linux/transferir-archivos.md
[urlOpenssl]: https://github.com/newdigicash/apuntes/blob/master/linux/generar-csr-openssl.md
[urlKeytool]: https://github.com/newdigicash/apuntes/blob/master/java/util/genera-csr-keytool.md
[urlNamecheap]: https://www.namecheap.com/security/ssl-certificates/
[urlComodo]: https://ssl.comodo.com/
[urlGodaddy]: https://www.godaddy.com/web-security/ssl-certificate
[urlDigicert]: https://www.digicert.com/es/
[urlComodoStore]: https://comodosslstore.com/
[urlListaCA]: https://www.techradar.com/news/best-ssl-certificate-provider
[urlConverterFormato]: https://www.sslshopper.com/ssl-converter.html
[urlCertbot]: https://certbot.eff.org
