---
title: Cómo saber si tus visitantes usan Ad-Block
description: "Hace unos días, visitando la web makeuseof me apareció un mensaje en la parte superior de la página en la que indicaba que se había detectado que estaba usando el plugin Ad-Block (Que bloquea la publicidad de las web, para quien no lo sepa). Y de una manera muy educada sugerían que se desabilitara para apoyar la web y así poder seguir ofreciendo contenido libre de costo."
layout: post.amp

categories:
  - How To
  - internet
tags:
  - adblock
  - anuncios
  - javascript
modified: 2016-02-07T09:33:22+01:00
main-class: "articulos"
color: "#F57C00"
---

[<img  title="adblock-plus-logo" src="/assets/img/2012/07/adblock-plus-logo11.png" alt=""  />][1]  
Hace unos días, visitando la web <a href="http://www.makeuseof.com/" target="_blank">makeuseof</a> me apareció un mensaje en la parte superior de la página en la que indicaba que se había detectado que estaba usando el plugin Ad-Block (Que bloquea la publicidad de las web, para quien no lo sepa). Y de una manera muy educada sugerían que se desabilitara para apoyar la web y así poder seguir ofreciendo contenido libre de costo.

Últimamente existe una obsesión muy grande por la privacidad en internet, cosa que apoyo, aunque creo que no hay que ser extremista.

Yo uso ad-block desde hace mucho tiempo, aunque no para todas las webs, makeuseof desde ahora es una más de la mi lista blanca en Ad-Block.

Personalmente pienso que en los blogs y páginas que leemos habitualmente es recomendable desabilitar este tipo de plugins, ya que es una forma de apoyar al propietario, que tanto tiempo y esfuerzo dedica a escribir y mantener la web y de paso, puede que nos resulte útil algún anuncio que se nos muestre y de paso le ayudamos a pagar el hosting del sitio. Por supuesto esto queda bajo el criterio de cada persona.

Es por eso que hoy voy a explicar cómo es posible detectar si nuestros visitantes tienen activado ad-block para nuestro sitio y así sugerirle de manera educada y sin molestarle demasiado si desea desactivarlo.  

<!--ad-->


Buscando cómo hacer esto me encontré con plugins para wordpress que bloquean al visitante que esté usandolo, lo cual no me parece correcto. Así que continué buscando y encontré un script que hace exáctamente lo que andaba buscando, *Adblock detector*.

En su web oficial nos ofrecen dos tipos de implementaciones:

```javascript
<script type="text/javascript">
  function _enabled() {
    alert('detected');
  }

  function _disabled() {
    alert('not detected');
  }

  var _abdDetectedFnc = '_enabled';
  var _abdNotDetectedFnc = '_disabled';
</script>
<script type="text/javascript" src="http://adblockdetector.com/script.php"></script>
```

y

```javascript
<script type="text/javascript">

  function _status(isDetected) {
    alert('detected: '+isDetected);
  }

  var _abdStatusFnc = '_status';
</script>
<script type="text/javascript" src="http://adblockdetector.com/script.php"></script>
```

Basta con modificar el contenido de las funciones **_enabled()** y **_disabled()** en el primer caso por el contenido que deseemos, o **_status** en el segundo método y listo.

Y a vosotros, ¿qué os parece que se os sugieran este tipo de cosas?, ¿Lo desactivaríais? dejad vuestros comentarios.

&nbsp;

Vía | <a href="http://adblockdetector.com/" target="_blank">Adblockdetector</a>



 [1]: /assets/img/2012/07/adblock-plus-logo11.png

{% include toc.html %}