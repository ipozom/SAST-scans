# Descubrimiento Tecnico para la Adopcion de SAST

## Resumen Ejecutivo

Este documento recoge el descubrimiento tecnico inicial y la premisa de implementacion requeridos para implementar escaneos de Static Application Security Testing (SAST) en un portafolio de aplicaciones que incluye Java/Kotlin, JavaScript/TypeScript, Python y C#/.NET. La audiencia prevista es el liderazgo de ingenieria y de seguridad.

El entregable actual cubre descubrimiento mas preparacion para un escaneo inicial de SAST que genere un reporte. No recomienda un proveedor especifico ni define un plan completo de despliegue empresarial. Su objetivo es establecer los criterios de evaluacion tecnica, las restricciones operativas, los requisitos de reporting, los riesgos clave y las decisiones de liderazgo necesarias antes de la ejecucion inicial.

En esta etapa, la direccion confirmada es la siguiente:

- la estrategia de SAST debe mantenerse neutral respecto a proveedores
- el portafolio de aplicaciones abarca multiples lenguajes de programacion
- el entorno de CI/CD es mixto y no esta estandarizado en una sola plataforma
- una modalidad de entrega basada en SaaS es aceptable
- las expectativas de cobertura de OWASP Top 10 y CWE deben enmarcar la discusion de seguridad
- el primer hito de ejecucion debe ser un escaneo inicial de SAST que produzca un reporte sobre repositorios seleccionados

La brecha principal no es conceptual. Es de evidencia. La organizacion aun necesita un inventario verificado de repositorios, sistemas de compilacion, flujos de CI/CD, limites de ownership, consumidores del reporte y controles actuales del SDLC seguro antes de poder iniciar una fase creible de escaneo inicial, implementacion mas amplia o seleccion de herramientas.

## Objetivo del Documento

Este documento tiene como finalidad:

- definir el descubrimiento tecnico y operativo necesario para la adopcion de SAST
- explicar las consideraciones multilenguaje y multiplataforma que afectan la viabilidad de la implementacion
- definir las condiciones minimas requeridas para implementar un escaneo inicial de SAST y generar un reporte
- identificar las decisiones que el liderazgo debe tomar antes de la ejecucion inicial
- establecer los limites de lo que se incluye en esta fase y lo que se difiere

## Requisito de Escaneo Inicial de SAST

El primer hito de implementacion debe ser un escaneo inicial de SAST que genere un reporte en lugar de un despliegue completo e inmediato. Se espera que el escaneo inicial valide la ejecucion tecnica, la calidad de los resultados y el flujo de reporting antes de que la organizacion se comprometa con una adopcion mas amplia.

El escaneo inicial debe disenarse para:

- ejecutarse sobre un numero limitado de repositorios representativos en lugar de cubrir todo el portafolio
- medir la duracion del escaneo, la calidad de la senal y el impacto en el flujo de trabajo de desarrollo en condiciones reales de CI
- generar un reporte que pueda ser consumido por liderazgo, AppSec y stakeholders de implementacion
- confirmar que los hallazgos pueden exponerse, triarse, suprimirse y resumirse de extremo a extremo
- producir una recomendacion clara sobre si escalar, ajustar o detener

## Alcance Confirmado y Supuestos

### Entradas Confirmadas

Los siguientes insumos de descubrimiento ya estan confirmados:

| Tema | Posicion actual |
| --- | --- |
| Audiencia | Liderazgo de ingenieria y liderazgo de seguridad |
| Tipo de entregable | Descubrimiento, evaluacion y premisa de implementacion de escaneo inicial |
| Postura frente a herramientas | Neutral respecto a proveedores |
| Lenguajes en alcance | Java/Kotlin, JavaScript/TypeScript, Python, C#/.NET |
| Postura de CI/CD | Plataformas multiples o mixtas |
| Supuesto de alojamiento | Las herramientas SaaS son aceptables |
| Marco de seguridad | Cobertura de OWASP Top 10 y CWE |
| Objetivo inicial de ejecucion | Escaneo inicial de SAST con generacion de reporte |

### Supuestos de Trabajo

Los siguientes supuestos se utilizan para orientar el descubrimiento hasta que se recopilen los datos del portafolio:

- la organizacion opera mas de un repositorio o servicio, en lugar de una sola base de codigo homogenea
- al menos una parte del portafolio utiliza control de cambios basado en pull requests
- distintos equipos pueden tener sistemas de compilacion diferentes o niveles distintos de madurez en pipelines
- el liderazgo tendra que equilibrar la cobertura de seguridad frente al impacto en el flujo de trabajo de desarrollo
- la primera implementacion debe mantenerse limitada al alcance de un escaneo inicial antes de un despliegue mas amplio

Estos supuestos deben validarse durante la fase de recopilacion de evidencia.

## Requisitos de Descubrimiento del Estado Actual

El analisis del estado actual esta incompleto hasta que se recopile y verifique la siguiente informacion.

### Inventario de Repositorios y Aplicaciones

Evidencia requerida:

- lista de repositorios o aplicaciones dentro del alcance
- lenguaje principal y framework por repositorio
- criticidad del repositorio o nivel de prioridad
- tipo de servicio, como API, aplicacion web, proceso batch o libreria
- ownership por equipo o dominio de negocio
- idoneidad para inclusion en el alcance del escaneo inicial

Por que esto importa:

- la profundidad del escaneo, los frameworks soportados y el esfuerzo de ajuste varian segun el lenguaje y el tipo de aplicacion
- la planificacion de la adopcion depende de entender donde debe comenzar la cobertura de mayor riesgo o mayor valor

### Contexto de Build y Dependencias

Evidencia requerida:

- herramienta de build por repositorio, como Maven, Gradle, npm, pnpm, yarn, pip, Poetry, NuGet o MSBuild
- requisitos de restauracion de paquetes y cualquier registro privado
- presencia de monorepos, codigo generado o librerias compartidas
- duracion promedio del build y causas comunes de inestabilidad
- capacidad para ejecutar el escaneo inicial en runners de CI o entornos con contenedores ya existentes

Por que esto importa:

- algunos enfoques de SAST requieren contexto de build o analisis semantico completo para producir resultados utiles
- los repositorios con builds fragiles o lentos generan mayor friccion durante el despliegue

### Panorama de CI/CD

Evidencia requerida:

- plataformas en uso, como GitHub Actions, GitLab CI, Jenkins o Azure DevOps
- patrones de validacion en pull requests frente a jobs por rama o programados
- politicas de bloqueo actuales en pipelines
- patrones de retencion de artefactos y publicacion de resultados
- mecanismo disponible para publicar el reporte del escaneo inicial o los artefactos de resultados sin procesar

Por que esto importa:

- un entorno de CI/CD mixto favorece patrones de orquestacion portables en lugar de implementaciones especificas por plataforma
- la postura de enforcement depende de donde se exponen los resultados del escaneo dentro del flujo de entrega y de como se actua sobre ellos

### Linea Base de Seguridad y Gobierno

Evidencia requerida:

- requisitos actuales de SDLC seguro
- herramientas existentes de escaneo de codigo o quality gates
- modelo de ownership para remediacion
- proceso de excepciones o dispensas
- requisitos de reporting para liderazgo o auditoria
- owner de soporte del escaneo inicial, coordinacion de triage y revision del reporte

Por que esto importa:

- la adopcion de SAST fracasa si existen hallazgos sin un modelo operativo claro para triage, ownership y manejo de excepciones

## Requisitos de Alcance y Reporting del Escaneo Inicial

La documentacion debe soportar explicitamente la implementacion de un escaneo inicial de SAST y la produccion de un reporte. Eso requiere mas que un descubrimiento generico. Requiere una definicion clara del alcance del escaneo, de las expectativas del output, de los criterios de exito y de los guardrails operativos.

### Criterios para Seleccionar el Escaneo Inicial

- el repositorio se mantiene activamente y tiene un owner claro
- el pipeline de CI es lo bastante estable como para medir el impacto del escaneo de forma confiable
- la base de codigo es representativa de una combinacion relevante de lenguaje o framework dentro del portafolio
- el equipo esta dispuesto a participar en triage y retroalimentacion durante el escaneo inicial
- el pipeline puede publicar resultados o artefactos sin workarounds mayores especificos de plataforma

### Requisitos del Reporte

- el reporte debe resumir repositorios escaneados, fecha de ejecucion, alcance del escaneo y estado del resultado
- el reporte debe incluir conteos por severidad y confianza cuando esten disponibles
- el reporte debe distinguir hallazgos brutos, probables verdaderos positivos, falsos positivos confirmados y elementos que requieren seguimiento
- el reporte debe anotar restricciones de ejecucion, duracion del escaneo y cualquier fallo o exclusion
- el reporte debe ser utilizable tanto por stakeholders tecnicos como por liderazgo como artefacto base

### Criterios de Exito del Escaneo Inicial

- el escaneo se ejecuta de forma confiable en el flujo de CI seleccionado
- los resultados pueden recopilarse y ensamblarse en un reporte utilizable
- los hallazgos se exponen con categorias de severidad y confianza que resulten utiles
- la duracion del escaneo se mantiene dentro del presupuesto de tiempo acordado
- el equipo participante puede revisar y triar resultados dentro de su flujo normal de trabajo
- el escaneo inicial produce una recomendacion clara sobre si escalar, ajustar o detener

### Guardrails Operativos del Escaneo Inicial

- comenzar sin bloqueo salvo que el liderazgo apruebe explicitamente una postura mas estricta
- limitar el alcance inicial a repositorios representativos en lugar de cobertura sobre todo el portafolio
- definir un owner para configuracion, revision de resultados y distribucion del reporte
- preservar la estabilidad de entrega definiendo una ruta de contingencia si el escaneo falla o produce ruido inaceptable
- acotar el escaneo inicial en el tiempo para que los resultados se revisen rapido y las decisiones no se difieran indefinidamente

## Criterios de Evaluacion Tecnica

El enfoque futuro de SAST y el modelo de implementacion del escaneo inicial deben evaluarse contra los siguientes criterios tecnicos.

### Expectativas Transversales entre Lenguajes

Expectativas minimas en todo el portafolio:

- soporte confiable para los lenguajes dentro del alcance y los frameworks de uso comun
- capacidad para identificar problemas alineados con OWASP Top 10 y las clases de CWE relevantes
- soporte para analisis de flujo de datos o analisis tipo taint cuando los ecosistemas de lenguaje lo requieran
- salida que pueda publicarse en un formato portable como SARIF
- modos de escaneo utiles tanto para la evaluacion de linea base como para retroalimentacion en pull requests
- controles de politica que puedan distinguir severidad, confianza y categorias de reglas
- suficiente fidelidad de output para soportar un reporte util del escaneo inicial

### Consideraciones Especificas por Lenguaje

#### Java y Kotlin

Consideraciones clave:

- se necesita una fuerte comprension de frameworks para stacks empresariales comunes como Spring o servicios basados en Jakarta
- puede ser necesario un analisis acoplado al build para lograr una profundidad semantica util
- los monolitos grandes o builds multi-modulo pueden convertir el tiempo de escaneo y la preparacion del entorno en factores materiales
- la precision importa porque un alto volumen de falsos positivos puede erosionar rapidamente la confianza en codebases grandes

Preguntas de descubrimiento:

- que versiones de Java y Kotlin estan en uso
- si los builds dependen de repositorios privados o configuracion especifica del entorno
- si el portafolio incluye codigo Android, servicios del lado del servidor o ambos

#### JavaScript y TypeScript

Consideraciones clave:

- el comportamiento dinamico del lenguaje incrementa el riesgo de calidad de analisis inconsistente entre herramientas
- los servicios front-end y Node.js pueden requerir distinta profundidad de reglas y distinto enfasis de amenaza
- los patrones de sanitizacion especificos de framework influyen en la tasa de falsos positivos para problemas como inyeccion o cross-site scripting
- las estructuras de monorepo y los package managers pueden complicar la deteccion de proyectos

Preguntas de descubrimiento:

- que frameworks estan en uso, como React, Angular, Next.js, Express o NestJS
- si estan dentro del alcance tanto JavaScript de front-end como de back-end
- como estan estructurados los monorepos y la gestion de paquetes basada en workspaces

#### Python

Consideraciones clave:

- los entornos interpretados requieren atencion especial a la resolucion de dependencias, supuestos de ejecucion y convenciones del framework
- la deserializacion insegura, la ejecucion de comandos, el renderizado de templates y el uso inseguro de librerias son areas de interes comunes
- el tipado inconsistente y los imports dinamicos pueden afectar la precision de resultados
- la gestion de entornos, como pip, Poetry o entornos virtuales, puede complicar los escaneos reproducibles

Preguntas de descubrimiento:

- que frameworks estan en uso, como Django, Flask o FastAPI
- si existen paquetes internos compartidos o patrones dinamicos de plugins
- como se crean los entornos de Python en los pipelines de CI

#### C# y .NET

Consideraciones clave:

- la cobertura de versiones de framework y runtime importa tanto en .NET Framework como en .NET moderno
- a menudo es necesario un analisis consciente del build para obtener resultados semanticos utiles
- los tipos de aplicacion ASP.NET, API, worker y escritorio pueden requerir prioridades de reglas diferentes
- la estructura de las soluciones y los feeds privados de paquetes pueden afectar materialmente la configuracion y estabilidad del escaneo

Preguntas de descubrimiento:

- que generaciones de runtime estan en uso en todo el portafolio
- si el portafolio incluye aplicaciones heredadas de .NET Framework
- como se gestionan la restauracion de paquetes y los builds de solucion en CI

## Patrones de Arquitectura de Integracion

Dado que el entorno de CI/CD es mixto, el diseno del escaneo inicial debe favorecer patrones portables en lugar de detalle de implementacion especifico por pipeline.

### Patrones Preferidos Independientes de la Plataforma

- escaneo basado en CLI que pueda ejecutarse en cualquier entorno de agente de build
- ejecucion del escaner en contenedores cuando se necesite portabilidad del entorno
- salida de resultados estandarizada, preferiblemente SARIF o un formato equivalente legible por maquina
- evaluacion de politicas guiada por API para que la logica de enforcement no quede codificada de forma diferente en cada sistema CI
- modos separados para escaneos de pull request, escaneos completos de linea base y reescaneos programados
- generacion de reportes a partir de outputs estandarizados o artefactos agregados del escaneo

### Guia de Implementacion del Escaneo Inicial

- comenzar con uno o pocos repositorios representativos en lugar de intentar una cobertura transversal inmediata
- preferir ejecucion sin bloqueo con publicacion de artefactos o dashboard en la primera iteracion
- mantener la configuracion lo suficientemente consistente entre repositorios del escaneo inicial para poder comparar el comportamiento
- validar la generacion del reporte dentro del flujo que el equipo participante ya utiliza
- definir que ocurre si el escaneo falla, supera el tiempo esperado o produce ruido excesivo antes de habilitarlo en pipelines compartidos

### Areas de Decision para la Integracion

La futura fase de implementacion tendra que elegir entre los siguientes patrones operativos:

| Area de decision | Opciones a evaluar | Preocupacion del descubrimiento |
| --- | --- | --- |
| Disparador del escaneo | Pull request, rama, programado, release | Cobertura frente a velocidad de retroalimentacion |
| Enforcement | Informativo, advertencia, bloqueo | Friccion para desarrollo frente a reduccion de riesgo |
| Destino de resultados | Logs de CI, interfaz de code scanning, dashboard centralizado | Visibilidad y accountability |
| Modelo de configuracion | Politica centralizada, configuracion local por repo, hibrido | Estandarizacion frente a flexibilidad para equipos |
| Estrategia para hallazgos heredados | Backlog completo, supresion de linea base, solo codigo nuevo | Viabilidad de la adopcion temprana |

## Consideraciones de Gobierno y Modelo Operativo

SAST no es solo una capacidad de escaneo. Es un modelo operativo. Por tanto, el descubrimiento debe evaluar como se poseeran, triaran y gobernaran los hallazgos.

### Temas de Gobierno Requeridos

- quien es propietario de la plataforma o servicio de SAST
- quien es responsable de los hallazgos y de los plazos de remediacion
- como se revisan y suprimen los falsos positivos
- como se aprueban y auditan las excepciones
- como recibe el liderazgo los reportes de cobertura y remediacion
- que umbrales de severidad y confianza son aceptables para enforcement

### Consideraciones para una Adopcion Temprana

Para un escaneo inicial, la organizacion deberia esperar una postura de enforcement por fases en lugar de un bloqueo estricto e inmediato en todos los repositorios. Esta es una premisa de implementacion inicial: la calidad de los primeros hallazgos y la madurez de los procesos de ownership suelen determinar si el bloqueo puede tener exito sin generar friccion excesiva.

## Riesgos y Tradeoffs

Los siguientes riesgos son materiales para un entorno multilenguaje y de CI mixto.

### 1. Calidad de Analisis Desigual entre Lenguajes

Las herramientas pueden ofrecer analisis semantico fuerte para un ecosistema y resultados mas debiles para otro. En consecuencia, una sola postura empresarial puede ocultar diferencias significativas en cobertura y tasas de falsos positivos.

### 2. Fatiga del Equipo de Desarrollo por Mala Calidad de Senal

Si los primeros escaneos generan grandes cantidades de hallazgos de bajo valor, los equipos aprenderan a ignorar los resultados. Este riesgo es especialmente alto cuando la salida del escaneo se introduce sin un modelo claro de severidad o una estrategia de linea base.

### 3. Impacto en Rendimiento y Confiabilidad del Pipeline

Los repositorios que requieren builds pesados o configuracion especifica del entorno pueden experimentar aumentos significativos en el tiempo de validacion. Ese riesgo se amplifica cuando existen multiples plataformas CI en uso y los entornos de agentes son inconsistentes.

### 4. Brechas de Gobierno

Si el ownership de remediacion, el manejo de excepciones y el reporting no estan definidos, los hallazgos se acumulan sin resolucion. En ese caso, la organizacion gana actividad de escaneo sin ganar reduccion real de riesgo.

### 5. Decisiones Prematuras Centradas en la Herramienta

Seleccionar una herramienta antes de entender el portafolio puede llevar a una cobertura deficiente por lenguaje, a integraciones complejas o a un modelo operativo que los equipos no pueden sostener.

## Brechas Estrategicas Identificadas

Al momento de redactar este documento, las siguientes brechas siguen abiertas:

- no hay un inventario verificado de repositorios adjunto a este documento
- no se ha validado la distribucion de lenguajes y frameworks con datos reales de repositorios
- no se ha capturado un inventario de sistemas de build
- no se ha documentado un mapa de plataformas de CI/CD por equipo o segmento del portafolio
- no se ha confirmado un modelo actual de ownership para remediacion
- no se ha elegido una estrategia de linea base para hallazgos existentes
- no se han nominado repositorios para el escaneo inicial ni equipos participantes
- no se ha definido la audiencia, el formato ni la ruta de distribucion del reporte

Estas brechas no bloquean la documentacion de descubrimiento, pero si bloquean un escaneo inicial defendible, un plan de implementacion mas amplio o una evaluacion formal de proveedores.

## Decisiones Requeridas del Liderazgo

El liderazgo debe usar este documento para orientar las siguientes decisiones.

### 1. Alcance del Escaneo Inicial

Que repositorios o grupos de aplicaciones deben participar en el escaneo inicial y que los hace lo suficientemente representativos como para informar una decision mas amplia?

### 2. Postura de Enforcement del Escaneo Inicial

Debe el escaneo inicial comenzar como informativo, basado en advertencias o bloqueante para niveles de severidad seleccionados?

### 3. Modelo de Ownership

Seran AppSec, ingenieria de plataforma o los equipos de aplicacion quienes posean el triage operativo y la responsabilidad de remediacion?

### 4. Audiencia y Formato del Reporte

Quien necesita consumir el reporte del escaneo inicial y que nivel de detalle se requiere para liderazgo, AppSec y equipos de ingenieria?

### 5. Tolerancia a la Friccion para Desarrollo

Que nivel de tiempo adicional de build, ruido en revisiones y sobrecarga de politicas es aceptable a cambio de detectar vulnerabilidades antes?

### 6. Mandato Posterior al Escaneo

Debe la organizacion avanzar hacia un despliegue mas amplio, hacia mas ajustes o hacia una evaluacion formal de producto despues de revisar el reporte del escaneo inicial?

## Actividades de Implementacion y Validacion del Escaneo Inicial

Una vez completada la evidencia minima de descubrimiento, el equipo de ejecucion debe validar la factibilidad implementando un escaneo inicial controlado de SAST en lugar de pasar directamente a un despliegue empresarial.

Actividades recomendadas para el escaneo inicial:

1. seleccionar repositorios representativos y confirmar equipos participantes y owners
2. implementar el escaneo inicial en el flujo de CI elegido usando una postura sin bloqueo salvo aprobacion explicita en otro sentido
3. probar si el escaneo produce hallazgos utiles alineados con las expectativas de OWASP y CWE
4. medir el impacto en la duracion de pull requests y escaneos completos frente a las lineas base actuales de pipeline
5. ensamblar y revisar el reporte generado con stakeholders de ingenieria, AppSec y liderazgo
6. revisar una muestra de hallazgos de alta severidad con ingenieros senior para estimar tasas de verdaderos positivos y falsos positivos
7. verificar que los flujos de triage, supresion, excepciones y distribucion del reporte estan operativos antes de considerar cualquier politica de bloqueo

## Siguientes Acciones Recomendadas

El siguiente paso inmediato es completar la evidencia minima necesaria para lanzar un escaneo inicial de SAST y generar un reporte.

Acciones recomendadas:

1. completar la matriz de evidencia con datos de repositorios, lenguajes, frameworks, build y CI/CD
2. nominar repositorios para el escaneo inicial y confirmar equipos participantes y owners
3. confirmar flujos de ownership, excepciones, consumidores del reporte y rutas de soporte con stakeholders de seguridad y plataforma
4. definir expectativas del reporte, metricas de exito, reglas de contingencia e impacto aceptable en pipeline antes de comenzar la ejecucion
5. ejecutar el escaneo inicial y usar el reporte para decidir un despliegue mas amplio, ajustes adicionales o una evaluacion formal de producto

## Limites de Alcance y Exclusiones

Este documento incluye requisitos para la implementacion del escaneo inicial y la generacion del reporte, pero excluye:

- comparaciones de proveedores o recomendaciones de producto
- runbooks completos de implementacion empresarial o ejemplos de CI para todas las plataformas
- secuenciacion del despliegue empresarial y fechas objetivo
- flujos detallados de remediacion
- analisis de licenciamiento o presupuesto

Estos temas deben tratarse en fases posteriores una vez que la evidencia de descubrimiento este completa, se haya revisado el reporte del escaneo inicial y el liderazgo haya confirmado la direccion operativa.

## Anexo A: Resumen del Estado de la Evidencia

| Area | Estado | Notas |
| --- | --- | --- |
| Audiencia y alcance del entregable | Confirmado | Enfocado en liderazgo, descubrimiento mas premisa de escaneo inicial |
| Lenguajes en alcance | Confirmado | Java/Kotlin, JavaScript/TypeScript, Python, C#/.NET |
| Postura de CI/CD | Confirmado | Se asume un entorno mixto |
| Aceptabilidad de SaaS | Confirmado | Se permiten herramientas SaaS |
| Marco de seguridad | Confirmado | OWASP Top 10 y CWE |
| Inventario de repositorios | Pendiente | Requiere recopilacion de datos del portafolio |
| Inventario de frameworks | Pendiente | Requiere revision de repositorios |
| Inventario de sistemas de build | Pendiente | Requiere insumos de ingenieria |
| Mapa de plataformas CI/CD | Pendiente | Requiere insumos de plataforma o DevOps |
| Modelo de gobierno | Pendiente | Requiere alineacion entre AppSec e ingenieria |
| Seleccion de repositorios para el escaneo inicial | Pendiente | Requiere alineacion entre liderazgo y equipos |
| Definicion del reporte del escaneo inicial | Pendiente | Requiere definir audiencia, formato y distribucion |