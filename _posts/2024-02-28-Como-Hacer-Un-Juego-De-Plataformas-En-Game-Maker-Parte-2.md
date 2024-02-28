---
title: Como hacer un juego de plataformas en Game Maker Parte 2
published: false
---


Después de 12 mil millones de fiesta pagana y siesta mundana, la cofradia ha decidido que debo actuar y subir parte 2 de esto XD

Lo qual es una lastima, yo quería seguir viendo anime y comer doriftos hasta rebentar como un ucraniano y luego inculpar al PSOE y a los marroquís

Y mira que hemos tenido buenos animes este 2023, como por ejemplo esta obra magna


<video width="600" height="600" controls>
  <source src="/assets/gamemaker2/squirrel_chan.mp4" type="video/mp4">
</video>

pero de la profundidad artistica de Ginga Eiyū Densetsu y de como ha convertido a toda una nación de adolescentes en nacionalsocialistas que juegan al hearts of iron y escuchan versiones nightcore de himnos militares y carameldansen ya hablaremos otro dia, ahora en serio

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