---
title: Como configurar LDAP en OpenSuse Leap 42.2 (Server) y OpenSuse Leap 15 (cliente) mediante YaST
published: false
---


Buenos dias gente, en este tutorial mostraré como configurar LDAP en OpenSuse, ¿Por que OpenSuse? ¿Por que no simplemente Ubuntu?

Bueno eso es porque OpenSuse tiene una herramienta llamada YaST la cual es una verdadera navaja suiza y puede que la mejor herramienta de configuración que hay para GNU/linux. Es por eso que en OpenSuse se configura LDAP de forma diferente al resto de distribuciones. Y dada la falta de tutoriales que hay en ello he decidido escribir uno yo mismo.

Hace poco intente hacerlo con la version mas reciente de Opensuse y escribirlo aqui pero debido a que habia muchas diferencias con la versión 42 he decidido modificar este tutorial y ya cuando aprenda a hacerlo en la version 15 la posteare en otro post

## 1. Configuración inicial

primero activaremos el firewall en el servidor

Abriremos yast

![](/assets/LDAP-opensuse/1.jpg)

iremos a cortafuegos y lo habilitaremos

![](/assets/LDAP-opensuse/firewall.jpg)
![](/assets/LDAP-opensuse/fire2.jpg)

Para este tutorial usaremos Opensuse Leap tanto en servidor como cliente, solo que el servidor estara en modo terminal mientras que el cliente tendra KDE Plasma

Usaremos estas IPs y el dominio captainlainist.site

* server -> 192.168.1.12

* cliente -> 192.168.1.13


Antes de nada cambiaremos los hostnames por server y cliente y pondremos las IPs


(las capturas que esten en modo terminal son las del servidor las que no son del cliente)

para ello abriremos yast

![](/assets/LDAP-opensuse/1.jpg)
![](/assets/LDAP-opensuse/10.jpg)

iremos a ajustes de red

![](/assets/LDAP-opensuse/2.png)
![](/assets/LDAP-opensuse/confred.jpg)

En el caso de que en el cliente no te deje editar nada ve a opciones globales y selecciona Servicio Wicked

![](/assets/LDAP-opensuse/wicked.jpg)

editar (si te fijas hay una letra resaltada en Editar (i) pulsando alt + esa letra y luego enter puedes acceder)

![](/assets/LDAP-opensuse/4.jpg)
![](/assets/LDAP-opensuse/editar_.jpg)

seleccionaremos dirección IP estatica y pondremos la configuracion que nos convenga

![](/assets/LDAP-opensuse/3.jpg)
![](/assets/LDAP-opensuse/ipp.jpg)

En ajustes de red, esta vez iremos a Nombre de Host/DNS

pondremos host, dominio, la IP del servidor en Servidor de Nombres 1 8.8.8.8 en Servidor de Nombres 2 y el dominio en Búsqueda de Dominio

![](/assets/LDAP-opensuse/newworld.jpg)
![](/assets/LDAP-opensuse/ip_client.jpg)

En encaminamiento habilitaremos el gateway

![](/assets/LDAP-opensuse/wachunga.jpg)
![](/assets/LDAP-opensuse/eee.jpg)

Aceptar

## 2. Instalación de paquetes

Iremos a Repositorios de software

![](/assets/LDAP-opensuse/7.jpg)
![](/assets/LDAP-opensuse/repositorios.jpg)


y activaremos todos los repositorios

![](/assets/LDAP-opensuse/6.jpg)
![](/assets/LDAP-opensuse/cosita.jpg)


Despues instalaremos estos paquetes en el servidor mediante zypper

```bash
sudo zypper install yast2-dns-server openldap2 krb5-server krb5-client yast2-auth-server bind yast2-auth-client
```

![](/assets/LDAP-opensuse/ewewe.jpg)


y en el cliente instalaremos krb5-client y yast2-auth-client

iremos a YaSt

Instalar software


![](/assets/LDAP-opensuse/install.jpg)

buscaremos el paquete y los instalaremos

![](/assets/LDAP-opensuse/aaaa.jpg)
![](/assets/LDAP-opensuse/konnichiwa.jpg)

## 3. Configuración DNS

Para poder hacer LDAP necesitamos usar un dominio

Reabriremos yast y iremos a Servidor DNS

![](/assets/LDAP-opensuse/yastdnsserver.jpg)


iremos a nuestra IP y iremos a siguiente

![](/assets/LDAP-opensuse/sigueinte.jpg)

añadiremos nuestro dominio como primario e iremos a editar

![](/assets/LDAP-opensuse/anyadir.jpg)

añadiremos los registros NS tanto del servidor como del cliente

![](/assets/LDAP-opensuse/dnsrecks.jpg)

luego iremos a los registros y añadiremos las ips y sus hostnames referentes

![](/assets/LDAP-opensuse/dnswew.jpg)


cuando aceptemos y le demos a siguiente hemos de recordar abrir el puerto del cortafuegos

![](/assets/LDAP-opensuse/corta.jpg)

y obviamente hay que configurarlo para que se inicie automaticamente

![](/assets/LDAP-opensuse/yqueinicie.jpg)

si probamos a hacer ping desde el cliente funcionara perfectamente

![](/assets/LDAP-opensuse/pingdns.jpg)



## 4. Configuración de certificados


en yast iremos al gestor de CA

![](/assets/LDAP-opensuse/cagestor.jpg)


crearemos una CA raiz


![](/assets/LDAP-opensuse/crearcaraiz.jpg)


Pondremos nuestros datos

![](/assets/LDAP-opensuse/nuestrosdatos.jpg)

y vamos a siguiente

Luego ponemos la contraseña de root que tambien sera la de LDAP


![](/assets/LDAP-opensuse/passwdmuybien.jpg)


Vamos a crear

![](/assets/LDAP-opensuse/crear.jpg)


Vamos a introducir CA poniendo nuestra contraseña

![](/assets/LDAP-opensuse/introducirCA.jpg)

Vamos a certificados y a añadir certificado de servidor

![](/assets/LDAP-opensuse/servcert.jpg)

rellenamos nombre común

![](/assets/LDAP-opensuse/rellenamos.jpg)

ponemos la contraseña de nuevo, siguiente y crear

![](/assets/LDAP-opensuse/siguienteycrear.jpg)

Volvemos a descripción, avanzados y exportar a fichero


![](/assets/LDAP-opensuse/wewe.jpg)


-Creamos la entidad

![](/assets/LDAP-opensuse/entidad.jpg)

En certificados vamos a exportar a fichero


![](/assets/LDAP-opensuse/certexport.jpg)

ponemos la contraseña y guardamos el archivo


![](/assets/LDAP-opensuse/estosdatos.jpg)

volvemos a exportar a fichero en certificados y esta vez guardamos la llave con estas configuraciones


![](/assets/LDAP-opensuse/volvemos.jpg)


## 5. Creación de servidor LDAP


Ahora en el servidor en YaST iremos a Servidor de autenticación


![](/assets/LDAP-opensuse/autenti.jpg)


Ponemos que el cortafuegos abra el puerto ldap


![](/assets/LDAP-opensuse/abrirport.jpg)


Dejamos Servidor autónomo

![](/assets/LDAP-opensuse/autonomo.jpg)


Ponemos los certificados creados anteriormente

![](/assets/LDAP-opensuse/ponemoslos.jpg)

ponemos el DN Base y la contraseña

![](/assets/LDAP-opensuse/dnbase.jpg)


No habilitamos kerberos

![](/assets/LDAP-opensuse/nokerb.jpg)


Y vamos a terminar

![](/assets/LDAP-opensuse/terminal.jpg)



## 6. Configuración de cliente LDAP

Para el cliente voy a continuar con la version 15 ya que no hay mucha diferencia

Vamos al yast a configuración del cliente LDAP

![](/assets/LDAP-opensuse/yast-ldap.jpg)


Vamos a cambiar configuración

![](/assets/LDAP-opensuse/cambiarconf.jpg)

Ponemos estas configuraciones

![](/assets/LDAP-opensuse/wiwi.jpg)

Y aceptar

iremos a acceso de usuarios en yast y activaremos estas configuraciones

![](/assets/LDAP-opensuse/anyadirdom.jpg)


añadimos el dominio ldap

![](/assets/LDAP-opensuse/23.jpg)


con estas configuraciones

![](/assets/LDAP-opensuse/24.jpg)


## 7.Creacion de usuarios

En yast vamos a gestion de usuarios y grupos

![](/assets/LDAP-opensuse/usergest.jpg)

En definir filtro pondremos usuarios LDAP

![](/assets/LDAP-opensuse/userldap.jpg)


y ponemos la contraseña de administrador LDAP

-Finalmente añadimos un usuario

![](/assets/LDAP-opensuse/danimal.jpg)


![](/assets/LDAP-opensuse/finalmenchi.jpg)


Y iniciamos sesión


![](/assets/LDAP-opensuse/pillao.jpg)



![](/assets/LDAP-opensuse/pillao2.jpg)


Y finalmente lo habremos hecho, Pasad una buena noche!! :D