---
id: 1348
title: 'Cómo configurar un servidor DNS &#8211; Parte 3 (Zona Inversa y DNS secundario)'

layout: post
guid: http://elbauldelprogramador.com/?p=1348
permalink: /como-configurar-un-servidor-dns3/
categories:
  - Administración de Servidores
  - Artículos
  - internet
tags:
  - A records
  - bind
  - bind tutorial dns
  - CNAME
  - como crear dns localhost
  - como crear una zona primaria en dns
  - configura un servidor dns
  - configuración de servidores
  - configuracion dns servidor dedicado
  - configuracion refresco zona dns
  - configuracion servidor dns
  - configuracion zona dns dominio
  - configurar dns
  - configurar servidor dns
  - debian
  - dns dedicado ovh
  - implementar servidor dns en debian
  - in-addr.arpa
  - MX records
  - named.conf
  - ovh
  - servidor debian
  - servidor dedicado ovh
  - servidor dns
  - servidor dns debian
  - servidor dns secundario ovh
  - servidores dns
  - soa correo
  - zona dns inversa
---
  * [Cómo configurar un servidor DNS &#8211; Parte 1 (Introducción)][1]
  * [Cómo configurar un servidor DNS &#8211; Parte 2 (La Zona Primaria)][2]
  * Cómo configurar un servidor DNS &#8211; Parte 3 (Zona Inversa y DNS secundario)

Ya se ha visto que existe una base de datos centralizada que asocia nombres de dominios a direccines IP, también se mencionó el caso inverso, una copia inversa de dicha base de datos, que asocia IP&#8217;s a nombres de dominios. Ésta búsqueda inversa es usada por muchos programas, que rechazarán establecer una conexión si la búsqueda inversa y la búsqueda directa (*Dominio»IP*) no coinciden. Muchos proveedores de correo usan la búsqueda inversa para clasificar correos como spam.

Con objetivo de que los emails enviados desde el dominio que se está configurando no sean clasificados como spam, es necesario crear la zona inversa en el archivo **named.conf.local**:

{% highlight bash %}zone "89.39.5.in-addr.arpa" {
 type master;
    file "pri.89.39.5.in-addr.arpa";
};
{% endhighlight %}

Los números son la dirección ip del servidor escritos en orden inverso. Es decir, la ip es **5.39.89.x**, así pues, la zona ha de llamarse *89.39.5.in-addr.arpa*. 

Es necesario crear el archivo de zone inversa también, *pri.89.39.5.in-addr.arpa*. Este archivo es necesario crearlo en el mismo directorio en el que se encuentra el archivo de zona primario (*pri.elbauldelprogramador.com*).

El principio de este archivo es exáctamente igual que *pri.elbauldelprogramador.com*:  
  
<!--more-->

{% highlight bash %}@       IN      SOA     ks3277174.kimsufi.com. contacto.elbauldelprogramador.com. (
                        2013021001       ; serial, todays date + todays serial #
                        7200              ; refresh, seconds
                        540              ; retry, seconds
                        604800              ; expire, seconds
                        86400 )            ; minimum, seconds
;
  NS      ks3277174.kimsufi.com.
  NS      ns.kimsufi.com.
{% endhighlight %}

A continuación, es necesario añadir un registro del tipo **PTR**. Los registros **PTR** son punteros. Apuntan a un nombre de dominio. Quedaría así:

{% highlight bash %}44   PTR elbauldelprogramador.com.
{% endhighlight %}

El 44 es el último valor de la dirección IP del servidor.

Eso es todo, en este punto usaremos el comando **dig** para comprobar la configuración.

{% highlight bash %}$ dig elbauldelprogramador.com

; <<>> DiG 9.8.4-P1 <<>> elbauldelprogramador.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 10156
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;elbauldelprogramador.com.    IN  A

;; ANSWER SECTION:
elbauldelprogramador.com. 532    IN  A   5.39.89.44

;; Query time: 50 msec
;; SERVER: 80.58.61.250#53(80.58.61.250)
;; WHEN: Mon Feb 11 21:09:28 2013
;; MSG SIZE  rcvd: 58
{% endhighlight %}

Así, estamos buscando la ip del dominio. Como se aprecia, devuelve el valor correcto en la sección **ANSWER SECTION**.

{% highlight bash %}$ dig -x 5.39.89.44

; <<>> DiG 9.8.4-P1 <<>> -x 5.39.89.44
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50347
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;44.89.39.5.in-addr.arpa.    IN  PTR

;; ANSWER SECTION:
44.89.39.5.in-addr.arpa. 84513 IN  PTR elbauldelprogramador.com.

;; Query time: 52 msec
;; SERVER: 80.58.61.250#53(80.58.61.250)
;; WHEN: Mon Feb 11 21:10:09 2013
;; MSG SIZE  rcvd: 76
{% endhighlight %}

Esta vez, se está realizando la petición inversa, preguntamos por el dominio.

### El servidor DNS secundario

En caso de disponer de otro servidor DSN propio, para configurarlo de modo que haga las veces de servidor DNS secundario es necesario añadir otra zona al archivo **named.conf.local** en el servidor **secundario**

{% highlight bash %}zone "DOMINIO" {
     type slave;
     file "sec.DOMINIO.COM";
     masters { DIRECCION IP SERVIDOR PRIMARIO; };
};
{% endhighlight %}

Esta vez, se declara la zona como **slave** o esclava y se especifica la dirección IP del servidor maestro. En el fichero indicado en **file** se almacenarán los datos de la zona esclava. Basta con reiniciar **named** y dicho fichero será creado al ponserse en contacto con el servidor primario y habiendo realizado una transferencia de zona.

Por último, por razones de seguridad es recomendable agregar una línea adicional en archivo de zona del servidor **principal** que únicamente permita al servidor secundario realizar la transferencia de zona:

{% highlight bash %}zone "elbauldelprogramador.com" {
        type master;
        allow-transfer {IP SERVIDOR DNS SECUNDARIO;};
        file "/etc/bind/pri.elbauldelprogramador.com";
};
{% endhighlight %}

En mi caso, la ip corresponde al servidor DNS secundario que proporciona la compañia en la que tengo contratado el servidor.

Con éste último artículo doy por terminada este conjunto de artículos que pretendían dar a conocer al lector el funcionamiento de DNS. Habiendo adquirido este conocimiento, será mucho más fácil usar las distintas herramientas que proporcionan los paneles de administración web para configurar los DNS.

Para finalizar, reiterar que todos los artículos están basados en un How to de la web que se menciona en las referencias.

#### Referencias

*Traditional DNS Howto* »» <a href="http://www.howtoforge.com/traditional_dns_howto" target="_blank">Visitar sitio</a> 



 [1]: /articulos/como-configurar-un-servidor-dns/ "Cómo configurar un servidor DNS – Parte 1 (Introducción)"
 [2]: /articulos/como-configurar-un-servidor-dns2/ "Cómo configurar un servidor DNS – Parte 2 (La Zona Primaria)"

{% include _toc.html %}