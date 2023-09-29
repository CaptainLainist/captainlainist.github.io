---
title: Tutorial Básico de nmap
published: true
---

Buenos días chicos que tal, como va todo, se que desde el tutorial lioso que hice de como hacer LDAP en Opensuse USANDO DOS PUÑETERAS VERSIONES DIFERENTES para cliente y servidor no he subido nada, he estado aprendiendo de unos cursos de hacking en udemy y no he tenido mucho tiempo, a parte estoy de vacaciones y quiero estar del chill.

Pero bueno para no alargar mas la cosa he decidido subir un tutorial de nmap

He aqui un mono con el que me identifico

![](/assets/nmap/monke.jpg)


# 1. Instalación

Nmap es una herramienta para escanear puertos muy util y con muchas funciones

En esta instalación usaremos Linux Mint aunque cualquier distribución basada en debian que use apt-get como instalador de paquetes deberia serviros

![](/assets/nmap/aptupdate.jpg)

primero actualizaremos los repositorios

```bash
sudo apt update
```

después instalaremos nmap

![](/assets/nmap/nmapinstall.jpg)

```bash
sudo apt install nmap
```

![](/assets/nmap/yalotengo.jpeg)

# 2. Ojo con la ley

Ojo, que usar nmap puede ser ilegal dependiendo del país que seas, asi que nada de hacer nmap a google.com y esas mierdas, para eso usaremos scanme.nmap.org una web donde legalmente es permitido hacer pruebas


# 3. Uso básico

Vale empecemos ahora en serio, primero hagamos un escaneo normal a scanme.nmap.org

```bash
sudo nmap scanme.nmap.org
```


![](/assets/nmap/escaneosimple.jpg)

como podemos ver nos detecta los puertos abiertos los cuales son

 * ssh
 * dns
 * http
 * nping-echo
 * Elite

 pero hay una pequeña cuestion, esos puertos en realidad no detecta el servicio, tansolo detecta el puerto y asigna un servicio al puerto default. Para saber que servicio corre usaremos -sV y para que solo asigne esos puertos usaremos -p

 En concreto se lo haremos al puerto ssh y Elite


 ```bash
sudo nmap -sV -p22,37331 scanme.nmap.org
```

![](/assets/nmap/sv_puertos.jpg)


Como podemos ver nos detecta la version y servicio de los puertos.

Gracias a eso ahora sabemos que el servicio Elite es en realidad tcpwrapped y que el puerto ssh usa Openssh 6.6.1

ahora probaremos a hacer un escaneo de solo TCP SYN que nos escaneara los puertos TCP de forma mas rápida y sigilosa con -sS,

usaremos todos los puertos posibles con -p- (ya que sin esto no los escanea todos),


y con --min-rate 5000 limitaremos la lentitud de los paquetes a 5000

```bash
sudo nmap -sS -p- --min-rate 5000 scanme.nmap.org
```

![](/assets/nmap/allports.jpg)


Vemos que salen los mismo puertos

con el parametro -T podemos medir la agrsividad del ataque, siendo -T0 muy lento y sigiloso y -T5 muy rapido y agresivo


```bash
sudo nmap -sS -p- --min-rate 5000 scanme.nmap.org -T5
```

![](/assets/nmap/agresivo.jpg)


Si queremos hacer un escaneo de UDP podemos usar el parametro -sU

```bash
sudo nmap -sU -p- --min-rate 5000 scanme.nmap.org -T5
```

![](/assets/nmap/udp.jpg)

Como podemos ver nos detecta dns y ntp

Con el parametro -F nos escaneara los puertos mas conocidos haciendo asi un escaneo rápido

```bash
sudo nmap -F scanme.nmap.org
```

![](/assets/nmap/rapido.jpg)

como podemos ver no escanea todos los puertos

Con top-port podemos escanear cierto numero de puertos de mas popular a menos

```bash
sudo nmap --top-ports 100 scanme.nmap.org
```

![](/assets/nmap/topports.jpg)

Asi escanea solo los 100 puertos mas populares


Si queremos guardar el resultado en un archivo xml podemos usar el parametro -oX

```bash
sudo nmap --top-ports 100 scanme.nmap.org -oX puertos.xml
```

![](/assets/nmap/puertosxml.jpg)

Asi quedaria el archivo

![](/assets/nmap/archivoxml.jpg)

Con el parametro --webxml podemos añadirle un contenido grafico xslt para hacer un reporte

```bash
sudo nmap --top-ports 100 scanme.nmap.org -oX puertos.xml --webxml
```

![](/assets/nmap/webxml.jpg)

Si lo abrimos con firefox quedaria así

![](/assets/nmap/reporte.jpg)



# 4. Detección de hosts


Para detectar hosts en una red, usaremos el parametro -sn para asi hacer un escaneo sin puertos y luego pondremos la red con su mascara de subred


```bash
sudo nmap -sn 10.5.50.0/24
```


![](/assets/nmap/detecciondehosts.jpg)

Muy importante hacer esta detección con sudo ya que asi escaneara por MAC y no por ip haciendo asi mas sigilosa y rapida la detección


# 5. estados de nmap

Antes de seguir vamos a estudiar los estados de nmap

 * open: el puerto esta abierto y hay un servicio corriendo
 * closed: el puerto esta abierto pero no hay un servicio escuchando
 * filtered: el firewall tiene cerrado el puerto y por lo tanto no podemos saber si hay un servicio corriendo
 * unfiltered: el firewall permite el puerto pero nmap es incapaz de saber si el puerto esta abierto o cerrado

# 6. Uso de scripts

Una cosa que tiene nmap es poder usar scripts

yendo a este directorio podemos listarlos

![](/assets/nmap/scripts.jpg)

y asi podemos ver que scripts hay


![](/assets/nmap/scriptsquehay.jpg)

Por ejemplo con este script podemos recibir informacion del servicio ntp

```bash
sudo nmap -sU -p123 --script=ntp-info scanme.nmap.org
```

![](/assets/nmap/utpinfo.jpg)


Con el script vuln podemos pasar todos los scripts de deteccion de vulnerabilidades aunque aviso que sera muy agresivo y facil de detectar ademas de tardar mucho

Se lo pasaremos solo al puerto 80 para que vaya mas rapido

```bash
sudo nmap -sS -p80 --script=vuln scanme.nmap.org -T5
```


![](/assets/nmap/vulns.jpg)

Otra cosa que podemos hacer por http muy util es pasar un fuzzer para descubrir directorios ocultos


```bash
sudo nmap -sS -T5 -p80 --script=http-enum scanme.nmap.org
```

![](/assets/nmap/ningunoxd.jpg)

Como podemos ver no ha detectado ningun directorio el pinche nmap

![](/assets/nmap/taco.jpeg)

Y bueno aunque hay mucho mas, eso es todo por hoy, siempre podeis marcaros un nmap --help para ver todas las opciones o ir a la web oficial para ver todos los scripts que podeis usar


En fin, hasta la proxima y tened un buen dia