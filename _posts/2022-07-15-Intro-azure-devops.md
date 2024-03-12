---
layout: post
title: "Notas de estudio: Continuous Testing with Azure DevOps"
date: 2022-07-15
excerpt: "Continuous Testing with Azure DevOps - TAU"
tags: [devops, azure, testing, tau]
comments: true
---

# Introduction to Azure Devops

## Azure

- Servicio de computación en la nube, creado por Microsoft para construir, testear, desplegar y administrar aplicaciones.

- Provee software como servicio (SaaS), plataforma como servicio (PaaS) y infraestructura como servicio (IaaS).

- Mas de 200 productos y servicios de nube, soporta varios lenguajes de programación, herramientas y frameworks.

- Puede crear un cuenta gratuita de Azure, la cual permite usar servicios sin cargo de por vida y algunos otros en los primeros 12 meses.

- Al igual que Azure por parte de Microsoft, existen otros proveedores de servicios de nube como AWS por parte de Amazon, Google Cloud Plataform de Google y OCI por parte de Oracle.

- Azure tiene un importante porcentaje de cuota de mercado de compañias Fortune 500.

## DevOps

- Surge de la unión de "desarrollo" y "operaciones".

- Dev incluye todas las personas involucradas en el desarrollo de producto software, por lo tanto, incluye a QA.

- Ops incluye al personal como administradores de sistemas, infraestructura, DBAs, ingenieros de redes, profesionales de seguridad y otras disciplinas relacionadas.

- Definición de DevOps que usa Microsoft captura la esencia misma que se pretende transmitir: "DevOps is the union of people, process, and technology to continually provide value to customers." (DevOps es la unión de personas, procesos y tecnologia para continuamente entregar valor a los clientes.).

- DevOps ofrece beneficios como acelerar el tiempo de comercialización de un producto, adaptarse rapidamente al mercado, mantener la estabilidad y confiabilidad de las aplicaciones y mejorar el tiempo de recuperación.

## [Azure DevOps](https://docs.microsoft.com/es-es/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)

- Suite de servicios de Azure para planear, colaborar y entregar mmas rápido productos software.

- Provee varias herramientas que cubren todo el ciclo de vida de desarrollo de producto y habilita capacidades DevOps.

Los servicios parte de Azure DevOps son:

- [Azure Boards](https://docs.microsoft.com/es-es/azure/devops/boards/get-started/what-is-azure-boards?view=azure-devops), para planear y hacer seguimiento al desarrollo de software, defectos, incidentes, usando Kanban y Scrum.
- [Azure Repos](https://docs.microsoft.com/es-es/azure/devops/repos/get-started/what-is-repos?view=azure-devops), para controlar y mantener el código fuente de las aplicaciones usando repositorio Git o TFVC (Team Foundation Version Control).
- [Azure Pipelines](https://docs.microsoft.com/es-es/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops) provee servicios para soportar integración continua (CI) y entrega continua (CD) de las aplicaciones.
- [Azure Test Plans](https://docs.microsoft.com/es-es/azure/devops/test/overview?view=azure-devops) ofrece varias herramientas para realizar pruebas manuales y exploratorias en las aplicaciones.
- [Azure Artifacts](https://docs.microsoft.com/es-es/azure/devops/artifacts/start-using-azure-artifacts?view=azure-devops) permite crear, hospedar y compartir paquetes de software con los equipos de desarrollo, también se pueden usar los artefactos en los pipelines de CI/CD.

- Estos servicios son independientes, por lo tanto, puede usarlos o no de acuerdo a sus necesidades.

- Adicional a estos servicios existe un mercado (marketplace) para Azure DevOps, para obtener plugins y extensiones que permiten personlizar y extender la experiencias en Azure DevOps.

Azure DevOps tiene dos ofertas:

- **Azure DevOps Services** es la oferta en la nube y proporciona un servicio hospedado escalable, confiable y disponible globalmente.
- **Azure DevOps Server** es la oferta para uso local (On-Premises) y se prefiere cuando desea que sus datos permanezcan dentro de su red.

Diferencias entre Azure DevOps Services y Azure DevOps Server: https://docs.microsoft.com/es-es/azure/devops/user-guide/about-azure-devops-services-tfs?view=azure-devops

## Documentación de Azure DevOps

https://docs.microsoft.com/es-es/azure/devops/?view=azure-devops
