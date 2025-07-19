# Rol

Eres un experto planificador de proyectos de software con Lean Agile

# Contexto

Estoy construyendo un sistema de tipo ATS. Ya tengo el PRD y definiciones técnicas de cara al desarrollo de dicho producto de software

# PRD de Sistema LTS

Ver documento completo: [PRD-SISTEMA-LTS.md](../docs/PRD-SISTEMA-LTS.md)

# Objetivo

## Roadmap de Producto

Clasifica y secciona el roadmap del producto en diferentes MVPs. Agrupando dentro de cada uno de estos MVPs, los hitos o épicas de desarrollo relacionadas. Organiza de mayor a menor prioridad, tomando como métrica: la entrega de valor al cliente y el product market fit temprano.

## Generar User Stories

Genera por favor 4 historias de usuario del MVP más importante considerando diferentes tipos de usuarios y sus necesidades. Las cuales están en el documento PRD entregado en la sección de contexto. Las historias de usuario, deben cumplir con el siguiente formato:

--

### Título de la Historia de Usuario

**Como** \[rol del usuario],
**quiero** \[acción que desea realizar el usuario],
**para que** \[beneficio que espera obtener el usuario].

#### Criterios de Aceptación

1. **Criterio #1**

   * **Dado que** \[contexto inicial]
   * **Cuando** \[acción realizada]
   * **Entonces** \[resultado esperado]

2. **Criterio #2**

   * **Dado que** \[contexto inicial]
   * **Cuando** \[acción realizada]
   * **Entonces** \[resultado esperado]

3. **Criterio #n**

   * **Dado que** \[contexto inicial]
   * **Cuando** \[acción realizada]
   * **Entonces** \[resultado esperado]

#### Criterios Técnicos

1. \[Criterio técnico #1]
2. \[Criterio técnico #2]
n. \[Criterio técnico #n]

#### Notas Adicionales

\[Cualquier consideración adicional]

#### Historias de Usuario Relacionadas

\[Relaciones con otras historias de usuario]

--

## Generar el Backlog del Producto

Considera el backlog de producto a partir de las historias de usuario creadas para el sistema LTS y estima por cada item en el backlog (genera una tabla markdown):

* Impacto en el usuario y valor del negocio.
* Urgencia basada en tendencias del mercado y feedback de usuarios.
* Complejidad y esfuerzo estimado de implementación.
* Riesgos y dependencias entre tareas.

Recuerda para este ejercicio, tener una columna adicional que agrupe cada historia de usuario, en su MVP correspondiente

## Generar Tickets de Trabajo

Toma la User Story más importante del primer MVP (más importante también) y genera una tabla con todos los tickets de trabajo necesarios para completar dicha User History, y añade tu sugerencia de estimación de puntos. Importante:

* Como metodología de asignación de puntos, utiliza fibonacci
* Dale un nombre e identificador a cada ticket de trabajo 
* Define a cada ticket de trabajo, una corta definición técnica para que el desarrollador sepa que iniciativas de desarrollo están involucradas