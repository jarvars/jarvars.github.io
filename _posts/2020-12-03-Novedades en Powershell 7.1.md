---
layout: post
title: "Novedades en Powershell 7.1"
date: 2020-12-03
excerpt: "Construida sobre .NET 5."
tags: [powershell, net5, microsoft, cmdlet]
feature: /assets/img/ps7Store.jpg
comments: true
---
Inicio revisando un poco la ruta de Powershell hasta el momento. Windows Powershell ha sido incluido desde Windows 7 SP1 y Windows Server 2008 R2 SP1, usando .NET Framework, versión tradicional de .NET, por lo que no es una versión multiplataforma, ni soporta las características de .NET Core. Luego, Microsoft lanzó PowerShell Core con el nuevo .NET Core, siendo una nueva versión multiplataforma y por lo tanto disponible en Windows, Linux y macOS. Microsoft ahora mantiene PowerShell Core como código abierto y está disponible en [GitHub](https://github.com/PowerShell/PowerShell). Estos cambios impulsan drásticamente el alcance de PowerShell como framework de automatización en múltiples plataformas y casos de uso, finalmente, separar Powershell de los lanzamientos de Windows le permite a Microsoft adoptar una postura mucho más agresiva para lanzar nuevas funciones y características de PowerShell, cada tres meses.

Las principales novedades en PowerShell 7.1 a revisar son:

- **Construido sobre .NET Core 5.0**

    Cada versión de PowerShell Core se correlaciona con una nueva versión de .NET Core. PowerShell 7.0 se basa en .NET Core 3.1 (LTS), ahora PowerShell 7.1 se basa en .NET 5.0. Al descargar versiones de PowerShell Core, se puede elegir entre ediciones con soporte a largo plazo (LTS) y versiones estables. PowerShell 7.1 está en la versión de rama estable y en la rama LTS se encuentra Powershell 7.0.3.  

- **Incluye PSReadLine 2.1.0**

    El autocompletado con Tab existe hace mucho tiempo en varios IDE de programación, esto permite escribir solo una parte de un comando, pulsar Tab y el comando completo se escriba automáticamente. Sin embargo, ahora existen cientos de cmdlets de PowerShell, incluidos los cmdlets de Azure PowerShell, por lo que completar con Tab, aunque es útil, no es tan efectivo como antes.

    Predictive Intellisense es la próxima evolución del autocompletado con Tab, ya que predice qué comando es posible que desee ingresar según el historial del cmdlet. Predictive Intellisense es posible gracias al módulo PSReadLine. Antes de PowerShell 7.1, era necesario descargar e instalar el módulo PSReadLine. Ahora, el módulo PSReadLine 2.1.0 viene con PowerShell 7.1, solo es necesario habilitarlo con el cmdlet:  

        Set-PSReadLineOption -PredictionSource History

    Luego de habilitar el módulo, cuando se escribe un cmdlet verá una sugerencia basada en Predictive Intellisense. Además, como se muestra en la imagen, el módulo PSReadLine no requiere instalación, ya que PowerShell 7.1 incluye el módulo de forma predeterminada.

    <figure>
    <img src="/assets/img/ps7Predictive.jpg">
    </figure>

- **Publicado en Microsoft Store**

    Ahora Microsoft Store ofrece la posibilidad de descargar PowerShell Core directamente desde la tienda. Antes del lanzamiento de PowerShell 7.1, era necesario descargar las nuevas versiones de PowerShell desde la [página oficial de PowerShell en GitHub](https://github.com/PowerShell/PowerShell). Para quienes ejecutan Windows 10, la incorporación de PowerShell como parte de la Tienda Windows permitirá encontrar, descargar e instalar PowerShell Core con menos esfuerzo, adicionalmente esto incluye que Microsoft Store permite realizar las actualizaciones disponibles junto con las actualizaciones regulares que se muestran en la tienda.

    <figure>
    <img src="/assets/img/ps7Store.jpg">
    </figure>

Finalmente, ademas de las novedades enumeradas anteriormente, Microsoft se ha centrado en resolver muchos problemas de la comunidad y corregir errores con la versión de PowerShell 7.1, incluyendo compatibilidad con Ubuntu 20.04, ya que no PowerShell 7.0 no lo era. 

Para ver una lista completa de las nuevas funciones específicas, las actualizaciones de cmdlet y otras mejoras en esta versión, ingrese a [https://docs.microsoft.com/en-us/powershell/scripting/whats-new/what-s-new-in-powershell-71?view=powershell-7.1](https://docs.microsoft.com/en-us/powershell/scripting/whats-new/what-s-new-in-powershell-71?view=powershell-7.1).