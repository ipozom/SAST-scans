# Matriz de Evidencia para el Descubrimiento de SAST

Esta matriz identifica la evidencia requerida para completar el descubrimiento tecnico del estado actual para la adopcion de SAST y para preparar la implementacion de un piloto de SAST. Su intencion es mantenerse como un documento de trabajo durante entrevistas con stakeholders y analisis de repositorios.

## Como Usar Esta Matriz

- capturar evidencia de propietarios de sistemas, equipos de plataforma o inspeccion de repositorios
- distinguir hechos verificados de supuestos
- identificar bloqueadores que impedirian la implementacion del piloto o una fase posterior de evaluacion de proveedores
- actualizar la confianza solo cuando la fuente de datos subyacente sea confiable y actual

Escala de confianza:

- `Confirmado` - verificado por una fuente autoritativa o por inspeccion directa
- `Alta` - fuertemente respaldado, pero aun no completamente conciliado
- `Media` - parcialmente respaldado o inferido
- `Baja` - solo un supuesto

Escala de estado:

- `Abierto` - aun se necesita evidencia
- `En progreso` - se esta recopilando
- `Cerrado` - recopilado y validado

## Insumos de Descubrimiento y Piloto

| Dominio | Dato | Por que se necesita | Owner sugerido | Fuente | Confianza | Estado | Notas |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Portafolio | Lista de repositorios o aplicaciones dentro del alcance | Define el limite real de cobertura | Liderazgo de ingenieria | Export de SCM o inventario del portafolio | Baja | Abierto | Insumo minimo para definir el alcance posterior |
| Portafolio | Criticidad de negocio o tier de aplicacion por repositorio | Soporta la adopcion por fases y la priorizacion | Owners de aplicacion | Catalogo de servicios o input del owner | Baja | Abierto | Necesario antes de seleccionar un piloto |
| Portafolio | Ownership del equipo por repositorio | Establece el enrutamiento de remediacion y excepciones | Managers de ingenieria | CODEOWNERS, catalogo de servicios, mapa de equipos | Baja | Abierto | Las brechas de ownership son un riesgo para el despliegue |
| Portafolio | Repositorios piloto candidatos y criterio de seleccion | Define el alcance inicial del piloto | Liderazgo de ingenieria y owners de aplicacion | Seleccion de stakeholders y revision de repositorios | Baja | Abierto | Elegir repositorios representativos y mantenidos activamente |
| Lenguajes | Lenguaje principal por repositorio | Determina la cobertura requerida del analizador | Ingenieria de plataforma | Inspeccion de repositorios | Baja | Abierto | Debe alinearse con la lista de lenguajes dentro del alcance |
| Lenguajes | Frameworks en uso | Determina necesidades de reglas conscientes del framework | Equipos de aplicacion | Inspeccion de repositorios, manifests | Baja | Abierto | Capturar frameworks de servidor y cliente por separado cuando sea necesario |
| Build | Herramienta y version de build | Determina prerrequisitos de analisis | Ingenieria de plataforma | Archivos de build y configuracion de CI | Baja | Abierto | Incluir dependencias en feeds privados de paquetes |
| Build | Duracion promedio del build | Estima la tolerancia al impacto en pipeline | Ingenieria de plataforma | Metricas de CI | Baja | Abierto | Capturar por separado duraciones de PR y de build completo si es posible |
| Build | Presupuesto aceptable de tiempo adicional para escaneo | Establece opciones viables de enforcement | Liderazgo de ingenieria e ingenieria de plataforma | Entrevista con stakeholders | Baja | Abierto | Necesario antes de fijar expectativas de escaneo en PR |
| Build | Capacidad para ejecutar escaneres en runners de CI o contenedores existentes | Determina si el piloto puede ejecutarse sin nueva infraestructura | Ingenieria de plataforma | Inventario de runners de CI y pruebas tecnicas | Baja | Abierto | Incluir restricciones de imagen, contenedores y red |
| Build | Estructura de monorepo o multi-modulo | Afecta la deteccion de proyectos y la estrategia de configuracion | Ingenieria de plataforma | Inspeccion de repositorios | Baja | Abierto | Importante para portafolios de JS/TS y Java |
| CI/CD | Plataformas de CI/CD en uso | Determina requisitos de portabilidad | DevOps o equipo de plataforma | Inventario de plataformas | Baja | Abierto | Se espera un mapa de plataformas mixtas |
| CI/CD | Patron de validacion en PR | Determina donde puede aparecer retroalimentacion rapida | DevOps o equipo de plataforma | Proteccion de ramas y configuracion de pipelines | Baja | Abierto | Necesario para definir futuras opciones de disparadores de escaneo |
| CI/CD | Quality gates existentes | Muestra la tolerancia actual al bloqueo de builds | DevOps o equipo de plataforma | Configuracion de pipelines | Baja | Abierto | Incluir lint, tests, coverage y herramientas de calidad |
| CI/CD | Capacidad para publicar resultados del escaneo en PRs, artefactos o interfaz de code scanning | Determina la ruta de feedback del piloto | DevOps o equipo de plataforma | Configuracion de pipelines y capacidades de la plataforma | Baja | Abierto | Debe poder probarse en el flujo del piloto |
| Seguridad | Herramientas existentes de SAST, calidad de codigo o escaneo de seguridad | Evita duplicacion y revela madurez actual | AppSec | Inventario de herramientas y entrevistas con stakeholders | Baja | Abierto | Incluir tooling en IDE y pipelines |
| Experiencia de desarrollo | IDEs principales y flujo de code review | Determina donde deben mostrarse los hallazgos para una remediacion rapida | Enablement de ingenieria o equipos de aplicacion | Entrevista con stakeholders | Baja | Abierto | Importante para el futuro diseno del flujo de trabajo |
| Seguridad | Requisitos de reporting o salidas para auditoria | Determina el formato de resultados y las expectativas de gobierno | AppSec o compliance | Documentos de politica | Baja | Abierto | La compatibilidad con SARIF puede importar despues |
| Seguridad | Modelo de severidad y expectativas de SLA para remediacion | Enmarca el enforcement y el triage futuros | AppSec | Politica de seguridad | Baja | Abierto | Tambien deben registrarse practicas informales |
| Gobierno | Modelo de ownership de hallazgos | Evita vulnerabilidades sin owner | Liderazgo de seguridad y liderazgo de ingenieria | RACI, entrevistas de proceso | Baja | Abierto | Uno de los insumos no tecnicos mas importantes |
| Gobierno | Proceso de falsos positivos y excepciones | Determina la sostenibilidad operativa | AppSec | Documentacion de procesos o input de stakeholders | Baja | Abierto | Requerido antes de cualquier postura bloqueante |
| Gobierno | Owner de soporte del piloto y ruta de escalamiento | Asegura que los incidentes del piloto puedan resolverse con rapidez | Ingenieria de plataforma y AppSec | Entrevista con stakeholders | Baja | Abierto | Definir quien responde ante fallos del escaneo o ruido excesivo |
| Hosting | Restricciones entre SaaS y self-hosted | Reduce las opciones de despliegue de herramientas | Liderazgo de seguridad | Politica o decision de arquitectura | Confirmado | Cerrado | SaaS es aceptable |
| Marco de seguridad | Expectativas de OWASP Top 10 / CWE | Define el objetivo base de cobertura | Liderazgo de seguridad | Decision de alcance | Confirmado | Cerrado | Aun no se ha confirmado un driver adicional de compliance |
| Preparacion para validacion | Repositorios candidatos para pruebas futuras de benchmark | Permite una prueba representativa de factibilidad | Liderazgo de ingenieria y AppSec | Seleccion de stakeholders | Baja | Abierto | Incluir al menos un repo por familia de lenguaje cuando sea viable |
| Piloto | Metricas de exito y criterios de salida del piloto | Define el umbral de avanzar o no escalar | Liderazgo de seguridad y liderazgo de ingenieria | Decision de stakeholders | Baja | Abierto | Incluir tiempo de escaneo, calidad de senal y cierre de triage |
| Piloto | Postura de contingencia si el escaneo del piloto falla o genera demasiado ruido | Evita interrupciones de entrega durante el piloto | Ingenieria de plataforma y AppSec | Entrevista con stakeholders | Baja | Abierto | La postura inicial por defecto debe ser sin bloqueo |

## Guia de Entrevistas con Stakeholders

Usar estas preguntas cuando se recopile la evidencia faltante.

### Liderazgo de Ingenieria

- Que aplicaciones o repositorios deberian cubrirse primero?
- Como deberia definirse la criticidad para el despliegue inicial?
- Que nivel de friccion para desarrollo es aceptable durante la adopcion temprana?
- Que repositorios y equipos deben participar en el piloto?
- Que resultados definiran el exito del piloto?

### Equipos de Plataforma o DevOps

- Que plataformas de CI/CD, agentes de build y plantillas de pipeline estan actualmente en uso?
- Que repositorios tienen builds fragiles o tiempos de validacion inusualmente largos?
- Como se gestionan actualmente credenciales compartidas, artefactos y resultados de escaneo?
- Pueden los runners o contenedores actuales soportar la ejecucion del escaner sin nueva infraestructura?
- Donde se publicaran los resultados del piloto para consumo del equipo de desarrollo?

### Equipos de Aplicacion

- Que frameworks, paquetes privados o patrones de codigo generado hacen que los builds no sean estandar?
- Existen areas conocidas que podrian producir hallazgos ruidosos o de bajo valor?
- Quien deberia ser owner de las decisiones de remediacion por cada repositorio?
- Esta disponible el equipo para participar en triage y retroalimentacion durante el piloto?
- Que contingencia se necesita si el piloto genera demasiada friccion en la entrega?

### AppSec o Liderazgo de Seguridad

- Que requisitos de reporting, excepciones y auditoria debe soportar cualquier modelo futuro de SAST?
- Que umbrales de severidad serian significativos para futuras decisiones de politica?
- Es el objetivo inicial la visibilidad, la reduccion de riesgo o eventualmente el enforcement de builds?
- Que calidad de senal es aceptable para considerar exitoso el piloto?
- Quien aprueba supresiones y decisiones de tuning durante el piloto?

## Criterios de Salida del Descubrimiento y Preparacion del Piloto

La fase de recopilacion de evidencia debe considerarse completa cuando:

1. todos los repositorios o grupos de aplicaciones dentro del alcance estan identificados y tienen owner
2. los datos de lenguaje, framework, build y CI/CD estan confirmados para el alcance inicial del despliegue
3. las practicas actuales de gobierno y manejo de excepciones estan documentadas
4. los repositorios piloto, owners del piloto y rutas de feedback del piloto estan identificados
5. las metricas de exito del piloto y la postura de contingencia estan definidas explicitamente
6. el liderazgo tiene suficiente evidencia para decidir si avanzar hacia la ejecucion del piloto, hacia una evaluacion de proveedores o hacia una planificacion de implementacion mas amplia