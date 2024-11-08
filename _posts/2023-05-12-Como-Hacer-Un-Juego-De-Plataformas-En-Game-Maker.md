---
title: Como hacer un juego de plataformas en Game Maker
published: true
---

Buenos días gente, ¿como va todo? Hoy vamos a apartarnos un poco de temas de sistemas para centrarnos en desarrollo de videojuegos con Game Maker.

# 1. Introducción

Para este tutorial usaremos Game Maker Studio 2, la qual es gratuita pero si quieres exportar tu juego tienes que pagar mensualmente por cada més que quieras poder exportarlo.

# 2. Creación de sprites

Lo primero que deberemos crear son los sprites, un sprite es basicamente una textura que se usa para juegos 2D

![](/assets/gamemaker/sprite.png)

Iremos a la carpeta de sprites y crearemos un sprite

![](/assets/gamemaker/edit_image.png)

Ahora editaremos el sprite y dibujaremos a nuestro personaje

![](/assets/gamemaker/monigote.png)


Ahora duplicaremos el sprite para que mire hacia el otro lado, para hacerlo seleccionaremos al sprite, pulsaremos Ctrl+X para cortarlo

![](/assets/gamemaker/duplicar.png)


![](/assets/gamemaker/seleccionar.png)

luego pulsaremos este boton para girarlo horizontalmente

![](/assets/gamemaker/girar.png)

y clicaremos donde queramos que se ponga el nuevo sprite

![](/assets/gamemaker/otro_lao.png)


Ahora crearemos los sprites de cuando corre

![](/assets/gamemaker/corre1.png)

para añadir un frame debemos pulsar ese botón marcado con un + y luego dibujar el nuevo frame

![](/assets/gamemaker/corre2.png)

Como solo tenemos dos frames, pondremos pocos fps en la animación del personaje, en este caso 5

![](/assets/gamemaker/frames.png)

De paso pondremos todos los sprites del personaje en colision fullimage y rectangle, el area de colisión será el area del sprite que aplicará código cuando choque con el area de colisión de otro objeto, es decir en jerga gamer es la hitbox.

![](/assets/gamemaker/fullimage.png)


En ese tono grisaceo oscuro podemos ver cual será el area de colisión

![](/assets/gamemaker/grisaceo.png)

Cabe decir que tener la misma area de colisión en un mismo objeto en todos los sprites es muy importante (en la mayoria de casos), ya que si tenemos diferentes areas de colisión es muy probable que nuestro personaje se quede atrapado en el suelo o las paredes.

![](/assets/gamemaker/otro_corre.png)

Finalmente duplicaremos el sprite para que corra hacia el otro lado

Ahora crearemos un ultimo sprite para crear el suelo, en mi caso importaré esta imagen.

![](/assets/gamemaker/dirt_minecraft.png)

Importaremos la imagen con el botón import

![](/assets/gamemaker/import.png)

Clicaremos sobre ese boton para asignar un tamaño adecuado


![](/assets/gamemaker/maximizar.png)

![](/assets/gamemaker/sesentaycuatro.png)

finalmente cambiaremos el nombre de los sprites

![](/assets/gamemaker/cambiar.png)

# 3. Creación de objetos

Ahora crearemos los objetos

Crearemos nuestro primer objeto así

![](/assets/gamemaker/create_obj.png)

Le pondremos de nombre player y el sprite que tendrá por defecto

![](/assets/gamemaker/object_player.png)

Luego crearemos el bloque de la misma manera

![](/assets/gamemaker/block.png)

Ahora añadiremos el evento Step al player, que será un evento que se ejecutará cada frame

![](/assets/gamemaker/add_event.png)

Nos pedirá si queremos usar Drag and Drop visual o GML, usaremos Visual ya que también podemos meterle código de esa forma

![](/assets/gamemaker/visual.png)

## 3.1 Gravedad

Añadiremos el bloque Object at place, que es para descubrir si hay un objeto en una localización en concreto o no, y marcaremos estas opciones.

![](/assets/gamemaker/object_at_place.png)

 * Object será block porque será lo que detendrá la gravedad
 * x será x porque (x == x) siempre es cierto y a nosotros lo que nos importa es la coordenada y
 * y sera 1 para añadirsele justo debajo de el personaje, luego le pondremos relative que viene a decir que se suma a si mismo, por ejemplo, sin relative seria y = 1, mientras que en relative seria y = y + 1
 * finalmente marcamos not para invertir la condición, es decir esta condición se crearà cuando no esté el objeto debajo


Ahora añadiremos el evento Execute code que nos permetra ejecutar codigo, en este caso si la condición anterior se cumple

![](/assets/gamemaker/gravedad.png)

```python
/// gravedad
gravity_direction = 270
gravity = 0.5
```

este código dice que la dirección de la gravedad será 270 (es decir abajo) y la potencia de la gravedad serà 0.5, la gravedad se diferencia de la velocidad vertical en que va creciendo por cada frame. Pero hay que limitarla, por eso modificaremos el codigo y pondremos esto

![](/assets/gamemaker/limitar_velocidad_vertical.png)

```python
/// gravedad
gravity_direction = 270
gravity = 0.5

//limitar velocidad vertical
if (vspeed >= 12){
	vspeed = 12
}
```

Esto limitará la velocidad vertical a 12.

ahora añadiremos un else a la condición

![](/assets/gamemaker/else2.png)

![](/assets/gamemaker/else.png)

Y pondremos que no haya gravedad

![](/assets/gamemaker/gravity_no.png)

```python
/// gravedad no
gravity = 0
```

else significa en el caso de que no se cumpla lo contrario lo que significa que se ejecutará cuando tengamos un bloque debajo.

Pero hay algo crucial en todo esto: que pasa si la velocidad vertical es alta y al caer sobre el suelo no caemos justo encima de él sino con los pies metidos dentro, ya que imaginemos que estamos 3 pixels sobre el bloque y la velocidad vertical es 12, para el proximo frame no caeremos sobre el bloque justo a 0, sino a (3-12) que son -9. Para eso solucinaremos con el siguiente evento.

# 3.2 Colision con el bloque

Crearemos el evento de colision con el bloque

![](/assets/gamemaker/collison_block.png)

Y añadiremos este codigo

![](/assets/gamemaker/codigo_colision.png)

```python
/// collision
//colision desde arriba
if (y+sprite_height > other.y && y < other.y){
	y = other.y-sprite_height
//desde abajo
} else if (y < other.y+other.sprite_height && y+sprite_height >  other.y+other.sprite_height){
	y = other.y+other.sprite_height
}
```

la primera condicion checkeará si el jugador tiene los pies undidos en el bloque pero la cabeza fuera, y asignara la posicion que debería tener, la de abajo hara lo contrario es decir si venimos de abajo.

Tengamos en cuenta que si una variable no tiene un objeto delante con un punto se refiere al propio objeto si es other.variable es referente al objeto con el que hace colision en los eventos de colision y si es obj_1.variable se refiere a todos los objetos obj_1 en la room



# 3.3 Movimiento

Ahora crearemos el movimiento

Crearemos el evento left de keyboard down en el player, el qual será la flecha izquierda en el teclado

![](/assets/gamemaker/left.png)

cabe decir que existen 3 diferentes tipos de eventos de pulsaciones de teclado

 * Key Down - Mientras mantengamos pulado la tecla
 * Key Pressed - Justo cuando la pulsamos
 * Key Up - Justo cuando dejamos de pulsarla

volveremos a añadir el bloque if object at place, y miraremos esta vez si la localizacion x-4 tiene algun bloque de colision

![](/assets/gamemaker/object_left.png)

Si no tiene ningun bloque de colision en x-4 añadiremos este bloque

![](/assets/gamemaker/jump_to_point.png)

el cual nos teletransportará a x-4, hay que tener en cuenta que en los juegos no existe el movimiento solo teletransportes a pequeña escala por frame, asi si no existe ningun bloque en x-4 nos moveremos hacia x-4

Ahora duplicaremos el evento y haremos que sea KeyDown Right

![](/assets/gamemaker/duplicar_evento.png)

En ese evento cambiaremos los -4 por 4, es decir que en vez de ser x-4 será x+4

![](/assets/gamemaker/cuatro.png)


# 3.4 Salto

Ahora añadiremos el evento Key Pressed Up al player y añadiremos el If Object at place que ya conocemos para checkear si hay un bloque debajo, luego en el caso de que si haya, asignaremos la variables vspeed (la velocidad vertical) a -12 contra mas se aleje el numero negativo de cero más potencia habrá de salto

![](/assets/gamemaker/vspeed.png)

# 3.5 Cambio de sprites

Volveremos un momento al left y right del player, primero al left

Añadiremos el bloque Set Sprite, pondremos el sprite de correr hacia la izquierda y pondremos el frame image_number en 0 y en relative, luego añadiremos un set animation speed para asignar la velocidad del sprite, pondremos 0.5

![](/assets/gamemaker/sprite_left_corre.png)

Haremos lo mismo en el key down right

![](/assets/gamemaker/sprite_right_corre.png)

Pero si cambiamos el sprite de correr mientras corremos cuando paremos de correr el sprite seguirá igual y por eso debemos añadir estos eventos

Key up left

![](/assets/gamemaker/keyupleft.png)

y key up right

![](/assets/gamemaker/keyupright.png)

a los cuales pondremos que el sprite sea el quieto

# 4. Creación de la room

Crearemos una room

![](/assets/gamemaker/room.png)

Y le pondremos los objetos

![](/assets/gamemaker/room2.png)

Finalmente iremos a play

![](/assets/gamemaker/play.png)

Y podremos probar nuestra demo

![](/assets/gamemaker/final.png)

# 5. Fin

Más adelante haré una parte 2 de esta demo. Hasta la proxima!