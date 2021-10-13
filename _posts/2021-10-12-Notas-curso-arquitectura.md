---
layout: post
title: "Notas de estudio curso Arquitectura de Software"
date: 2021-10-12
excerpt: "Curso Profesional de Arquitectura de Software - Platzi"
project: true
tags: [arquitectura, software, devops, automation]
comments: true
---

# Contenido
- [Contenido](#contenido)
  - [Atributos de Calidad](#atributos-de-calidad)
    - [Idoneidad funcional](#idoneidad-funcional)
    - [Eficiencia de ejecución](#eficiencia-de-ejecución)

## Atributos de Calidad

Los atributos de calidad son las expectativas de usuario, en general implícitas (pueden ser explicitas), de cuán bien funcionará un producto.

### Idoneidad funcional

Este concepto agrupa 3 atributos de calidad, habla de cómo una funcionalidad cumple con las necesidades del usuario. Se puede medir a través de una conexión entre los requerimientos y la aceptación, se puede hacer mediante acuerdos de aceptación de los requerimientos.

Completitud funcional
: Cuán completa esta la implementación con respecto a lo que se espera del sistema.   
*Requerimientos Funcionales vs Funcionalidades implementadas.*

    **Ejemplo**: Login con redes sociales. ¿Cuáles? (implementación incremental).

Exactitud funcional
: Cuán preciso es el sistema para implementar lo requerido.  
*Resultados Esperados vs Resultados Obtenidos.*

    **Ejemplo**: Reportes históricos. Límites de análisis por volumen de datos.

Pertinencia funcional
: Cuán alineado esta lo que se implementó con lo requerido.  
*Objetivos Cumplidos vs Objetivos Esperados.*

    **Ejemplo**: Sistemas CRUD en su evolución

### Eficiencia de ejecución

Esta característica representa el desempeño en relación con la cantidad de recursos utilizados en las condiciones establecidas. Esta característica se compone de los siguientes atributos que deben ser comparados entre sí.

Tiempo a Comportamiento
: Indica cuán bueno es el sistema respondiendo al usuario; Específicamente cuánto tarda el sistema y cuanto esperamos que ese sistema tarde.

    **Ejemplo**: Latencia en videojuegos online en cada acción.

Uso de recursos
: Grado en que las cantidades y tipos de recursos utilizados por un producto o sistema, en el desempeño de sus funciones, cumplen con los requisitos. Cuanto el sistema aprovecha esos recursos en su contexto, sea RAM, SO, etc. El objetivo es saber cuan bien o mal se están usando.

    **Ejemplo**: Reportes con grandes volúmenes de datos. 

Capacidad
: Grado en el que los límites máximos de un producto o parámetro del sistema cumplen los requisitos. 
    **Ejemplo**: Sistemas de tickets, de compras en línea por temporada.