---
title: Como hacer un juego de plataformas en Game Maker Parte 2
published: true
---


Después de 12 mil millones de fiesta pagana y siesta mundana, la cofradia ha decidido que debo actuar y subir parte 2 de esto XD

Lo cual es una lastima, yo quería seguir viendo anime y comer doriftos hasta rebentar como un enano en la fiesta mayor y luego inculpar al partido político de turno y a los de fuera

Y mira que hemos tenido buenos animes este 2023, como por ejemplo esta obra magna


<video width="600" height="600" controls>
  <source src="/assets/gamemaker2/squirrel_chan.mp4" type="video/mp4">
</video>

pero de la profundidad artistica de Ginga Eiyū Densetsu y de como ha convertido a toda una nación de adolescentes en edgelords elitistas que juegan al hearts of iron y escuchan versiones nightcore de himnos militares franceses y carameldansen ya hablaremos otro dia, ahora en serio

# 1. Enemigos

Vamos a crear un enemigo, easy verdad? un simple y llano goomba, aunque en verdad será algo mucho más terrorifico

![](/assets/gamemaker2/machismo.png)

Efectivamente el horrible e inhumano machismo

Crearemos el objeto, ya sabeis como hacer esto

![](/assets/gamemaker2/machismo2.png)

y añadiremos el evento create al cual le añadiremos este código para que se mueva hacia la izquierda siempre


![](/assets/gamemaker2/evento_mover.png)

Luego añadiremos colision con el bloque, y le añadiremos este código para que invierta la velocidad horizontal

![](/assets/gamemaker2/invertir.png)

Ahora añadiremos la colision con el player y le añadiremos este codigo

![](/assets/gamemaker2/codigo1.png)

```python
/// colision player
if (instance_position(self.x+self.sprite_width/2, self.y-10, other)){
	other.vspeed = -6
	instance_destroy()
} else {
	room_restart()
}
```

Ahora procedere a explicar este código, que solo se ejecutará cuando el enemigo colisione con el player, depende de como colisione, deberemos o destruir al enemigo o reiniciar la room (game over), entonces para saber si el player esta matando al enemigo, solo debemos saber si esta arriba del enemigo

![](/assets/gamemaker2/explicacion.png)

la funcion instance_position(x, y, obj), detecta si hay algun objeto en esa localizcacion

 * x sera self.x+self.sprite_height/2 = self, se refiere al propio objeto (el enemigo) mientras que other sería el objeto con el que colisiona (el player), entonces self.x se le sumara la mitad de su anchura (self.sprite_width/2),

 * cogeremos la y  pero un poco más arriba (y-10),

 * finalmente escogeremos con que objeto esta chocando (el player, que sera aqui other)

 * podriamos poner player, en vez de other, pero entonces afectaria a todos los players, y no con el que colisiona, no deberia pasar nada, porque sólo hay un player, pero si hubiera más, habría problemas con ello

Cuando pisoteemos al enemigo, el player debe dar un pequeño salto, por eso pondremos other.vspeed = -6, que hará que la velocidad vertical del player sea -6, lo cual es hacía arriba, pero no procuparse, que la gravedad ya assignada hara que solo de un saltito, y después de eso, borrará el objeto del enemigo con instance_destroy()

Sí hay colisión pero por los lados, la room actual se reiniciará, como un game over

![](/assets/gamemaker2/enemigo.png)

finalmente ponemos al enemigo en la room, y veremos que funciona

Y ya de paso haremos que cuando el player se caiga se reinicie la room de nuevo

![](/assets/gamemaker2/outside.png)

Para eso añadiremos el evento outside room

![](/assets/gamemaker2/restart.png)

y ponemos restart room para reiniciar la habitación

# 2. Cámara que sigue al jugador

Ok todo eso de los enemigos mola mucho y tal, pero y si el nivel es demasiado grande para ocupar la pantalla, aa cagaste primo, caiste en el viejo truco de no usar viewport

lo primero será agrandar el tamaño de la habitación lateralmente, para eso iremos a la room y agrandaremos el tamaño lateral a 10000


![](/assets/gamemaker2/lateral.png)



Ahora haremos un nivel algo más adiente, recuerda que pulsando ALT + clic izquierdo puedes poner los objetos más facilmente, y que con shift y moviendo la rueda del ratón puedes moverte facilmente a través del nivel


![](/assets/gamemaker2/nivel.png)


Ahora para configurar las viewports iremos donde la room a Viewports and Cameras

![](/assets/gamemaker2/viewports.png)

Habilitaremos las primeras dos opciones para habilitar los viewports y para limpiar su fondo

despues en el viewport 0, pondremos visible, la localizacion de camera y viewport, podemos dejarlas tal cual, serán las coordenadas del tamaño de la pantalla cortada, y en horizontal border, pondremos 600 para que la pantalla empiece a moverse más o menos a la mitad. Finalmente pondremos 4 para referirnos a la velocidad a la que ira el movimiento de la camara y pondremos como object following al player.

# 3. DeltaTime

Hay una cosa que hay que tener en cuenta, recordais que por cada frame pusimos que la velocidad del jugador fuera 4?, bueno pues que pasaría si el ordenador fuera muy lento, o muy rápido? pues que al haber más frames por segundo la velocidad variaria, una solucion a esto es multiplicar la velocidad por deltatime, que es el tiempo entre frame y frame, procederemos a hacer este cambio en estas capturas

En el player:

![](/assets/gamemaker2/deltatime.png)
![](/assets/gamemaker2/dt2.png)
![](/assets/gamemaker2/dt3.png)
![](/assets/gamemaker2/dt4.png)

como podeis ver, a cada instancia de velocidad aplicamos este algoritmo

```python
velocidad = velocidad_adecuada * delta_time * room_speed * 0.000001
```

en el caso del enemigo, como empieza a moverse al primer frame, cuando aun es 0, no podemos aplicarlo tan facilmente, por eso hay que aplicarlo como pone en estas capturas

![](/assets/gamemaker2/dt5.png)
![](/assets/gamemaker2/dt6.png)
![](/assets/gamemaker2/dt7.png)
![](/assets/gamemaker2/dt8.png)


# 4. Disparos

Ahora aprenderemos a hacer disparos, vamos lo divertido

![](/assets/gamemaker2/jp.png)


Creamos este sprite y objeto

![](/assets/gamemaker2/fireball.png)

y le añadimos colision con el enemigo

![](/assets/gamemaker2/colision_ene.png)

y añadimos el siguiente código

```python
/// destruir
instance_destroy(other)
instance_destroy()
```

asi destruiremos al enemigo y después a la bala

Después haremos que se destruya al impactar un bloque

![](/assets/gamemaker2/destroy.png)

y que se destruya al salir de la room (para ahorrar memoria)

![](/assets/gamemaker2/outside_2.png)

Ahora crearemos la variable direccion en cuanto vayamos a la izquierda que será -1

![](/assets/gamemaker2/direccion1.png)

y haremos lo mismo para la derecha pero siendo 1

![](/assets/gamemaker2/direccion2.png)


![](/assets/gamemaker2/direccion3.png)

pondremos también esto en el create, ya que antes de moverse tambien mira hacía algun lado

ALERTA: No hagais que la variable se llame direction, porque esa variable ya existe dentro de game maker, llamadla de otra manera, como direccion (en castellano)

De esta manera, hemos asignado una variable a que direccion esta mirando el jugador

ahora, al pulsar la z pondremos este codigo

```python
/// disparar
var bala = instance_create_layer(x + (30*direccion), y,  "Instances", fireball)
bala.hspeed = (direccion*7) * delta_time * room_speed * 0.000001
```

![](/assets/gamemaker2/disparar.png)

Procedere a explicar el código

 * primero crearemos una variable llamada bala, que guardara el ultimo disparo creado, lo guardaremos de la función instance_create_layer, que crea un objeto en un layer
 * el primer valor es la coordenada x, donde se guardará. Para que no quede en el medio, y quede un poco hacia delante del jugador, le sumaremos 30 pero multiplicandolo por direccion, ya que si mira hacía la izquierda será 30 * -1 que es igual a -30, y si mira hacia la derecha será 30 * 1 que es 30. Después pondremos la y, que será la del jugador. 
 * Luego la layer donde se pondrá, solo hay una layer por defecto de objetos, que es "Instances"
 * finalmente el objeto, que sera el objeto fireball que hemos creado (o como hayais llamado al objeto del disparo)

 * después en la segunda linea habrá que ponerle la velocidad a la bala, por eso siguiendo la lógica de antes cogeremos 7 que será la velocidad y le multiplicaremos direccion, y luego le aplicaremos delta time

y finalmente tendremos los disparos

![](/assets/gamemaker2/juego_final.png)

# 5. Continuará

Bueno de momento el concilio estará contento con esto, continuaré en el futuro con una parte 3, aunque probablemente haga otra cosa antes de subirla.

Quería comentar que como este es mi blog y le pego patadas voladoras en la cara cuando quiero, puede que de vez en cuanto no suba tutoriales de informática, y tal vez suba alguna review de algo, algo que comentar no se, pero principalmente voy a ir subiendo tutoriales de informática.

y ahora sí me permitís debo influenciarme negativamente de todas las ideologias militaristas radicales con esta obra maestra

![](/assets/gamemaker2/ginga.png)