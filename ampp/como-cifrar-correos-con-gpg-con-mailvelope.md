---
title: Cómo cifrar correos electrónicos con GPG usando Mailvelope
layout: post.amp

categories:
  - seguridad
tags:
  - correos gpg
  - enviar correos cifrados
  - instalar mailvelope en chrome
  - instalar mailvelope en firefox
  - tutorial mailvelope
  - user gpg en correo
  - como cifrar correos electrónicos
image: 2013/04/GnuPG-Logo.png
introduction: "Cómo cifrar correos electrónicos con gpg y mailvelope"
description: "En estos tiempos en los que está claro que estamos sometidos a vigilancia de los gobiernos, es posible que queramos un poco de privacidad cuando nos comunicamos por la red. Hoy voy a explicar cómo configurar un plugin para Firefox y Chrome que nos permitirá enviar correos de forma segura mediante GPG, **Mailvelope**."
main-class: "articulos"
color: "#F57C00"
---

<figure>
  <img src="/assets/img/2013/04/GnuPG-Logo.png" alt="Cómo cifrar correos con GPG usando Mailvelope" />
</figure>

En estos tiempos en los que está claro que estamos sometidos a vigilancia de los gobiernos, es posible que queramos un poco de privacidad cuando nos comunicamos por la red. Hoy voy a explicar cómo configurar un plugin para Firefox y Chrome que nos permitirá enviar correos de forma segura mediante GPG, **Mailvelope**.

<!--ad-->

### Introducción

En otro artículo vimos [cómo cifrar archivos mediante GPG][1], en esta ocasión se trata de lo mismo, pero cifrando el contenido de correos electrónicos.

### Descargar e instalar Mailvelope en Chrome

Los usuarios de este navegador simplemente deben instalar Mailvelope como cualquier otro plugin en la siguiente <a href="https://chrome.google.com/webstore/detail/mailvelope/kajibbejlbohfaggdiogboambcijhkke?hl=en-US" title="Instalar Mailvelope en Chrome" target="_blank">dirección</a>.

### Descargar e instalar Mailvelope en Firefox

El plugin aún no está disponible de forma oficial para firefox, pero podemos usar su repositorio en [Git][2] para compilarlo e instalarlo. Los siguientes pasos se han extraído de la <a href="https://github.com/toberndo/mailvelope/tree/firefox#firefox" title="Compilar Mailvelope" target="_blank">documentación oficial</a>:

```bash
git clone git://github.com/mozilla/addon-sdk.git
cd addon-sdk
source bin/activate
cd ..
git clone git://github.com/toberndo/mailvelope.git
cd mailvelope
git checkout -t origin/firefox
git submodule init
git submodule update
make build
make dist-ff

```

Tras esto, en **dist/mailvelope.xpi** se encuentra el plugin para instalarlo.

### Generar llaves en Mailvelope

Una vez instalado mailvelope en el navegador correspondiente, hacemos **click en el icono del plugin » opciones**. Aparecerá un formulario que rellenaremos con un nombre, el correo a usar y un **passphrase**, en las opciones avanzadas podemos elegir el algoritmo de cifrado, la longitud y la fecha de expiración:

<img src="/assets/img/2013/08/Cómo-cifrar-correos-con-GPG-usando-Mailvelope.png" alt="Cómo cifrar correos con GPG usando Mailvelope"   />

### Enviar la clave pública a un servidor de llaves

El par de claves pública/privada que acabamos de generar debe aparecer en *Display keys*. La seleccionamos y hacemos click en **Export » Display public key**, copiamos la llave y la pegamos en la sección **Submission And Publication** del servidor de llaves <a href="http://keyserver.borgnet.us/" target="_blank">keyserver.borgnet.us</a>. A partir de ahora, cualquiera que tenga a su disposición la clave pública que acabamos de subir al servidor podrá enviarnos un correo cifrado.

### Ejemplo

Vamos a poner un ejemplo con la cuenta de correo de contacto de este blog. El primer paso es obtener la clave pública que se encuentra en la página de [contacto][3] o en este otro <a href="http://keyserver.borgnet.us:11371/pks/lookup?op=get&search=0x083EDE12BE101B2B" target="_blank">enlace</a>. La copiamos y en la sección **Import Keys** de Mailvelope la pegamos. Ahora mi clave pública se encuentra en tu anillo de claves.

Como es la primera vez que ambas cuentas de correo van a ponerse en contacto, para que yo pueda enviar correos cifrados debo conocer la clave pública del otro usuario. Mailvelope dispone de una opción que permite enviar la clave pública por correo en Display Keys » (Seleccionamos la clave deseada) » Export » Send Public Key by email. Si por algún motivo no funcionara simplemente copiamos la clave pública y la pegamos en el correo como parte del mensaje. Otra opción es proporcionar el enlace del servidor de claves donde reside.

<img src="/assets/img/2013/08/Cómo-cifrar-correos-con-GPG-usando-Mailvelope1.png" alt="Cómo cifrar correos con GPG usando Mailvelope"   />

Como vemos en la imagen, aparece un simbolo a la derecha, tenemos que pulsarlo y escribir en mensaje ahí:

[<img src="/assets/img/2013/08/Cómo-cifrar-correos-con-GPG-usando-Mailvelope2.png" alt="Cómo cifrar correos con GPG usando Mailvelope"   />][4]{.thumbnail}

Como aparece en la imagen, si es la primera vez que ambos correos se ponen en contacto, hay que enviar la clave pública para que la otra persona pueda reponder con un mensaje cifrado. Luego hacemos click en el candado y seleccionamos a clave pública con la que cifrar el mensaje, en este caso con la del correo de este blog, que hemos importado más arriba. Ya solo queda hacer click en **Transfer** y obtendremos esto:

<img src="/assets/img/2013/08/Cómo-cifrar-correos-con-GPG-usando-Mailvelope3.png" alt="Cómo cifrar correos con GPG usando Mailvelope"   />

Pulsamos enviar y listo.

El proceso contrario, es decir, cuando nos envíen un email cifrado es bastante intuitivo, abrimos el correo y nos encontramos con esto:

<img src="/assets/img/2013/08/Cómo-cifrar-correos-con-GPG-usando-Mailvelope4.png" alt="Cómo cifrar correos con GPG usando Mailvelope"   />

El cursor adaptará la forma de una llave, hacemos click, introducimos nuestro **passphrase** y descifraremos el mensaje.

Espero que haya sido de utilidad y os animéis a usar diariamente esta tecnología.



 [1]: https://elbauldelprogramador.com/editar-y-crear-archivos-cifrados-con-gpg-en-vim/ "Editar y crear archivos cifrados con GPG en Vim"
 [2]: https://elbauldelprogramador.com/mini-tutorial-y-chuleta-de-comandos-git/ "Git: Mini Tutorial y chuleta de comandos"
 [3]: https://elbauldelprogramador.com/ "Contacto"
 [4]: /assets/img/2013/08/Cómo-cifrar-correos-con-GPG-usando-Mailvelope2.png

{% include toc.html %}