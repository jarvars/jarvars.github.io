---
layout: post
title: ".Net 5: Creando una prueba unitaria NUnit con Visual Code"
date: 2020-12-18
excerpt: "...desde consola"
tags: [nunit, net5, unittest, vcode]
feature: /assets/img/nunit-net5/feature.jpg
comments: true
---
La idea es crear una prueba unitaria usando .Net 5, Visual Code y NUnit, ya que quiero empezar a probar lo que brinda .Net 5 y sus plantillas desde la terminal. Para llevarlo a cabo realizaré los siguientes pasos.

- Instalar el SDK de .Net 5.
- Probar .Net 5 desde la terminal.
- Crear y configurar un proyecto NUnit.
- Crear una prueba.
- Probar.

## Instalar el SDK de .Net 5

Para instalar el SDK de .Net 5 primero lo descargo desde la URL https://dotnet.microsoft.com/download/dotnet/5.0.

<figure>
<img src="/assets/img/nunit-net5/DownloadNet5.jpg">
</figure> 

Realizo la descarga para Windows en plataforma x64 de la columna **Build apps – SDK**, luego procedo con la instalación la cual es una instalación básica sin personalización, de está manera termino este paso.

## Probar .Net 5 desde la terminal

Voy a verificar en una consola de powershell que .Net 5 este disponible, consultando la versión de .Net instalada con el siguiente comando:

{% raw %}
dotnet –-version
{% endraw %}

<figure>
<img src="/assets/img/nunit-net5/dotnetVersion.jpg">
</figure> 

De esta manera valido la versión de .Net instalada y que funciona correctamente.

## Crear y configurar un proyecto NUnit

Listo, verificada la versión de .Net instalada, desde la misma consola voy a abrir Visual Code para empezar a trabajar en el directorio actual con el siguiente comando, tener el cuenta que el punto indicado como parámetro le indica a Visual Code que abra el directorio actual.

{% raw %}
code .
{% endraw %}

<figure>
<img src="/assets/img/nunit-net5/openVcode.jpg">
</figure>

Desde Visual Code puedo seguir trabajando con una consola de Powershell y en el contexto del directorio, desde el menú **Ver/Terminal** abro una consola de Powershell o por medio del atajo de teclado **Ctrl + ñ**. 

<figure>
<img src="/assets/img/nunit-net5/vsCodeTerminal.jpg">
</figure>

Voy a crear una solución, parte de la magia disponible en .Net 5 y desde las primeras versiones de .Net Core, es el uso de la CLI de .Net para realizar estas tareas desde consola. Listo, con el siguiente comando creo la solución de .Net, el parámetro **–n** indica el nombre que le asignaré a la solución:
{% raw %}
dotnet new sln -n 'NUnitTests'
{% endraw %}

<figure>
<img src="/assets/img/nunit-net5/vsCodeSolucion.jpg">
</figure>

Ahora que tengo una solución necesito crear el proyecto de prueba unitaria con NUnit, para esto, el equipo de NUnit dispone de una plantilla de proyecto que está incluida en el SDK de .Net, por lo tanto puedo crear el proyecto desde la CLI de .Net, y al igual que el comando de creación de la solución, el parámetro **–n** indica el nombre del proyecto.

{% raw %}
dotnet new nunit -n Net5.Tests
{% endraw %}

<figure>
<img src="/assets/img/nunit-net5/vsCodeProyecto.jpg">
</figure>

La plantilla incluye una clase de pruebas unitarias de ejemplo de NUnit para ayudar a iniciar la construcción de pruebas.

<figure>
<img src="/assets/img/nunit-net5/claseEjemplo.jpg">
</figure>

Y en el archivo de proyecto se visualiza las dependencias que incluye la plantilla de NUnit.

<figure>
<img src="/assets/img/nunit-net5/ArchivoProyecto.jpg">
</figure>

Ahora necesito vincular el proyecto a la solución creada anteriormente, con el siguiente comando:

{% raw %}
dotnet sln add Net5.Tests/Net5.Tests.csproj
{% endraw %}

<figure>
<img src="/assets/img/nunit-net5/AddProyecto.jpg">
</figure>

Ok, hasta acá he descargado e instalado el SDK de .Net 5, he creado desde la terminal de Visual Code una solución de .Net, un proyecto de prueba unitaria con la plantilla de NUnit y dicho proyecto lo he enlazado a la solución creada anteriormente, continuo con la creación de un tests básico.

## Crear una prueba

Voy a usar la clase creada por el ejemplo, pero primero voy a cambiarle el nombre ya que no es muy descriptivo, pero en realidad no tengo una aplicación para testear, entonces voy a crear un ejemplo básico, un método que realiza una suma, por lo tanto, la clase será nombrada como **CalculadoraTest**. Tener en cuenta que es una convención recomendada que la clase y el archivo de la clase tenga el mismo nombre.

<figure>
<img src="/assets/img/nunit-net5/renombraArchivo.jpg">
<figcaption>Renombrar el archivo.</figcaption>
</figure>

<figure>
<img src="/assets/img/nunit-net5/renombraClase.jpg">
<figcaption>Renombrar la clase.</figcaption>
</figure>

Finalmente, para testear incluiré un método privado que retorne la suma de 2 enteros e incluiré el método que realiza la prueba, con fines de realizar un test muy básico.

<figure>
<img src="/assets/img/nunit-net5/sumaTest.jpg">
</figure>

## Probar

Ya para finalizar ejecuto la prueba y reviso el resultado esperado, antes que nada debo compilar el proyecto y posteriormente ejecuto la prueba, ambos pasos los haré desde la consola con la intención de verificar y tener el acercamiento con los comandos respectivos, ya luego tendré tiempo de revisar entornos gráficos de Visual Code para explorar y ejecutar los tests.

Para compilar el proyecto con .Net 5, teniendo en cuenta que estoy ubicado en el mismo directorio de la solución:

{% raw %}
> dotnet build .
{% endraw %}

<figure>
<img src="/assets/img/nunit-net5/donetBuild.jpg">
</figure>

Y para finalizar, el comando para ejecutar la prueba construida, muy breve, muy fácil:

{% raw %}
> dotnet test
{% endraw %}

Tener en cuenta la salida del comando, indicando las pruebas detectadas y el resultado de las mismas de acuerdo a su estado, fallidas, completadas, ignoradas.

<figure>
<img src="/assets/img/nunit-net5/donetTest.jpg">
</figure>

Listo, un primer acercamiento muy breve y básico a .Net 5, el uso de la CLI del mismo, las plantillas de NUnit disponibles dentro del SDK y el uso desde un editor de código como Visual Code, de esta manera puedo agilizar estas tareas sin requerir algo mas robusto como Visual Studio.