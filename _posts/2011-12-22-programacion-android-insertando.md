---
id: 313
title: 'Programación Android: Insertando registros'

layout: post
guid: http://elbauldelprogramador.org/programacion-android-insertando-registros/
permalink: /programacion-android-insertando/
blogger_blog:
  - www.elbauldelprogramador.org
  - www.elbauldelprogramador.org
blogger_author:
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
  - Alejandro Alcaldehttps://profiles.google.com/117030001562039350135noreply@blogger.com
blogger_permalink:
  - /2011/12/programacion-android-insertando.html
  - /2011/12/programacion-android-insertando.html
share_data:
  - '[]'
  - '[]'
share_all_data:
  - '{"like_count":"0","share_count":"0","twitter":0,"plusone":2,"stumble":0,"pinit":0,"count":2,"time":1333551762}'
  - '{"like_count":"0","share_count":"0","twitter":0,"plusone":2,"stumble":0,"pinit":0,"count":2,"time":1333551762}'
share_count:
  - 0
  - 0
categories:
  - android
  - opensource
tags:
  - curso android pdf
  - Insertando registros
---
<div class="separator" style="clear: both; text-align: center;">
  <a href="/images/2013/07/iconoAndroid.png" imageanchor="1" style="clear:left; float:left;margin-right:1em; margin-bottom:1em"><img border="0" src="/images/2013/07/iconoAndroid.png" style="clear:left; float:left;margin-right:1em; margin-bottom:1em" /></a>
</div>

En la entrada anterior hablamos de cómo [obtener regístros][1] de los proveedores de contenidos (CV) usando [URIs][2]. Ahora vamos a ver como insertar registros.

Anroid utiliza una clase llamada *android.content.ContentValues* para retener los valores de un solo registro, que será el que se insertará. Los *ContentValues* son un dicionario de pares clave/valor, al igual que los nombres de columnas y valores de las [bases de datos][3]. La forma de insertar un registro es rellenando el ContentValues primero y despues decir a *android.content.ContentResolver* que lo inserte usando una URI.

<p class="alert">
  Necesitamos usar ContentResolver porque a este nivel de abstracción, no le estamos pidiendo a una base de datos que inserte un registro, estamos pidiendo insertar un registro en un CV identificado por una URI. El contentResolver es el responsable de resolver la referencia a la URI al proveedor correcto.
</p>

Abajo se muestra un ejemplo de cómo rellenar una fila con un ContentValues y prepararlo para un insert:

  
<!--more-->

{% highlight java %}ContentValues cv = new ContentValues();
cv.put("NombreColumna1", "valor1");
cv.put("NombreColumna2", "valor2");

//Ahora el objeto cv está preparado para insertarse en la BD
{% endhighlight %}

Podemos obtener una referencia al ContentResolver mediante un método de la clase Activity:

{% highlight java %}ContentResolver cr = miActivity.getContentResolver();
{% endhighlight %}

Todo lo que necesitamos en este punto es una URI para decirle al ContentResolver que inserte la fila. [En entradas anteriores][4] vimos que nuestra URI era:

{% highlight java %}content://com.elbauldelprogramador.provider.FavSites/sites{% endhighlight %}

Con esta URI y nuestro ContentValues podemos ahora hacer una llamada para insertar nuestra fila:

{% highlight java %}Uri uri = cr.insert(FavSitesProviderMetaData.CONTENT_URI, cv);
{% endhighlight %}

La línea anterior devuelve una uri apuntando al nuevo registro añadido, la URI devuelta se parecerá a esto:

{% highlight java %}FavSitesProviderMetaData.CONTENT_URI/Nuevo_ID{% endhighlight %}

Con esta Uri, podemos por ejemplo mostrar en el log del sistema qué registro hemos añadido:

{% highlight java %}Log.d("Nuevo registro añadido","Uri:" + insertedUri);
{% endhighlight %}

## Añadir archivos a un Content Provider

Puede que en ocasiones necesitemos añadir un archivo a una base de datos. Lo normal es guardar el archivo en disco y actualizar el registro en la base de datos para que apunte al archivo correspondiente. Android usa este protocolo y lo automatiza mediante la definición de un procedimiento específico para guardar y recibir estos archivos. Usa un convenio en el cual una referencia a un nombre de archivo se guarda en un registro con un nombre de columna reservado llamado **_data**.

Cuando se inserta un registro en esta tabla, se devuelve la uri. Una vez guardamos el registro mediante este mecanismo hay que realizar un seguimiento al archivo en esa dirección. Para hacer esto, Android permite al ContentResolver coger la URI del registro de la base de datos y devolver un writable output stream. Android asigna un archivo interno y almacena la referencia a ese nombre de archivo en el campo *_data.*

Para poder realizar estas operaciones necesitamos crear una columna adicional llamada _data, hacer una llamada al método *insert* para obtener una URI. EL siguiente código demuestra como se haría:

{% highlight java %}ContentValues cv = new ContentValues();
cv.put("NombreColumna1", "valor1");
cv.put("NombreColumna2", "valor2");

//Usamos el content resolver para insertar un registro
ContentResolver cr = miActivity.getContentResolver();
Uri nuevaUri = cr.insert(FavSitesProviderMetaData.CONTENT_URI, cv);
{% endhighlight %}

Una vez tenemos la uri del registro, pedimos a ContentResolver que obtenga una referencia al output stream del archivo:

{% highlight java %}//Usamos el ContentResolver para obtener  el output stream directamente
//El content resolver oculta el acceso al campo _data donde se almacena realmente la referencia al archivo
OutputStream outStream = miActivity.getContentResolver().openOutputStream(nuevaUri);
algunArchivoDeImagen.compress(Bitmap.CompressFormat.JPEG, 50, outStream);
outStream.close;
{% endhighlight %}

El código usa el flujo de salida para escribir.

* * *

#### Siguiente Tema: [Actualizar y borrar registros][5] {.referencia}





 [1]: /2011/12/programacion-android-usando-la-clausula.html
 [2]: /2011/11/programacion-android-proveedores-de.html
 [3]: /p/bases-de-datos.html
 [4]: /2011/11/programacion-android-arquitectura-de.html
 [5]: /programacion-android-actualizar-y/

{% include _toc.html %}