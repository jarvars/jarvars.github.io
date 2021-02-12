---
layout: post
title: "Robot Framework I"
date: 2021-01-18
excerpt: "La serie tutorial"
tags: [tutorial, robotframework, testing, automation, rpa]
feature: /assets/img/robotfw/robotfw.jpg
comments: true
---
Voy a estar escribiendo esta serie tutorial de Robot Framework mientras lo voy estudiando, de esa manera afianzo y comparto el conocimiento.

### Capítulos 

1. Introducción

# Robot Framework

Es un framework que permite la automatización pruebas de aceptación y de procesos robóticos (RPA), de código abierto publicado bajo la Apache License 2.0, fue desarrollado inicialmente por Nokia Networks y liberado como código abierto en 2008.

Es independiente del sistema operativo, se implementa con Python y también se ejecuta en Jython (JVM) e IronPython (.NET).

Al igual que Cucumber o Specflow en C#, los scripts son legibles por roles no técnicos, para integrar diferentes roles como desarrolladores, testers y managers. El Framework tiene un ecosistema amplio de bibliotecas y herramientas desarrollados como proyectos separados para extender su aplicación. 

El proyecto está completamente alojado en [GitHub](https://github.com/robotframework/robotframework) y se puede obtener por medio de [PyPi](https://pypi.python.org/pypi/robotframework).

Ejemplo básico de un script con Robot Framework

{% highlight robot linenos %}
*** Settings ***
Documentation     A test suite with a single test for valid login.
...
...               This test has a workflow that is created using keywords in
...               the imported resource file.
Resource          resource.robot

*** Test Cases ***
Valid Login
    Open Browser To Login Page
    Input Username    demo
    Input Password    mode
    Submit Credentials
    Welcome Page Should Be Open
    [Teardown]    Close Browser
{% endhighlight %}    

Para el siguiente capitulo estaré revisando la configuración del ambiente de desarrollo y de un proyecto inicial y cargarlo a un repo en GitHub.

Hasta pronto.
