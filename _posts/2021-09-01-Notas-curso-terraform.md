---
layout: post
title: "Notas de estudio sobre IaC con Terraform"
date: 2021-09-01
excerpt: "IaC - Infrastructure as Code"
project: true
tags: [terraform, iac, devops, automation]
comments: true
---

## Contenido
- [Cap 1: ¿Qué es infraestrutura como código?](#cap-1-qué-es-infraestrutura-como-código)
  - [Principios de IaC](#principios-de-iac)
  - [Prácticas generales de IaC](#prácticas-generales-de-iac)
- [Cap 2: Tipos de herramientas para implementar IaC](#cap-2-tipos-de-herramientas-para-implementar-iac)
  - [Herramientas para definicion de infraestructura](#herramientas-para-definicion-de-infraestructura)
  - [Herramientas para configuracion de servidores](#herramientas-para-configuracion-de-servidores)
  - [Enfoques para gestión de servidores](#enfoques-para-gestión-de-servidores)
  - [Factores para elegir una herramienta de IaC](#factores-para-elegir-una-herramienta-de-iac)
  - [Herramientas conocidas](#herramientas-conocidas)

# Cap 1: ¿Qué es infraestrutura como código?

Práctica de aplicar los principios de desarrollo de software para automatizar los procesos de creación infraestructura. 

## Principios de IaC

-   **La infraestructura se puede reproducir fácilmente**: dado que la creación de infraestructura se define en un script, reproducir la infraestructura es tan fácil como ejecutar nuevamente el script.

-   **La infraestructura es desechable**: si la infraestructura definida tiene una definición errónea, se puede desechar y recrearla nuevamente ajustando el script de definición.

-   **La infraestructura es consistente**: porque refleja lo que se define en su correspondiente script.

-   **La infraestructura es repetible**: la definición se encuentra como código en el script y se ejecuta n cantidad de veces.

-   **El diseño siempre está cambiando**: actualizar la infraestructura es más fácil.

## Prácticas generales de IaC

-   **Utilizar un archivo de definición**: todas las herramientas de IaC tienen un formato propio para definir infraestructura.

-   **Sistemas y procesos autodocumentados**: dado que todo se define en scripts, la infraestrctura es autodocumentada en los mismos.

-   **Versionar todo**: tal como se realiza en desarrollo de software, es importante versionar todos los cambios realizados en la infraestructura, esto permite mantener la traza, evaluar incidentes y retroceder en el tiempo.

-   **Preferir cambios pequeños**: mas como práctica de marcos de trabajo ágiles, es preferible realizar cambios pequeños para evitar conflictos mayores.

-   **Mantener servicios continuamente disponibles**: teniendo en cuenta que es muy fácil crear y recrear infraestructura, mantener los servicios de infraestructura siempre disponibles, es mas fácil.

# Cap 2: Tipos de herramientas para implementar IaC

En términos generales, estas herramientas se dividen en dos categorías:

## Herramientas para definicion de infraestructura

Permiten especificar que recursos de infraestructura crear y como deben configurarse. Los recursos de infraestructura pueden ser máquinas virtuales, interfaces de red, discos duros, plataforma como servicio. Los **archivos de definicion de configuracion** es donde se define la infraestructura a crear, todas las herramientas tienen archivos de configuración en diferentes formatos y lenguajes. Estos archivos son útiles para la automatizacion de la infraestructura.

Ejemplo archivo de configuración de Terraform:

{% highlight javascript %}
variable "example" {
default = "hello"
}

resource "aws_instance" "example" {
instance_type = "t2.micro"
ami           = "ami-abc123"
}
{% endhighlight %}  

## Herramientas para configuracion de servidores 

Permiten configurar los servidores con el estado deseado. Por ejemplo, instalar dependencias, crear directorios, usuarios, permisos, etc.

> **Aprovisionamiento**: proceso que permite que un elemento esté listo para usarse.

## Enfoques para gestión de servidores

-   Configuración de servidores desde un script

-   Empaquetar plantillas de servidores

-   Ejecutar comandos en servidores

-   Configuración desde un registro central

## Factores para elegir una herramienta de IaC

-   Modo desantendido para herramientas de líneas de comandos

-   Idempotencia, ejecución de comandos de manera repetida sin presentar un error

-   Parametrizable

## Herramientas conocidas

| Definición de Infraestructura | Configuración de Servidores |
| --                            | --                          |
| Terraform                     | Ansible                     |
| Cloud formation               | Chef                        |
| Open stack heat               | Puppet                      |

