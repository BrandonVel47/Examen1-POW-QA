# Examen 1: Pull Request con aseguramiento de calidad

Desarrollo de Aplicaciones Móviles Nativas | Grupo 7CV4 | Periodo 2027-1
Escuela Superior de Cómputo, IPN

## Equipo

| Integrante | Usuario de GitHub | PR |
|---|---|---|
| Brandon Velazquez Beltran | [BrandonVel47](https://github.com/BrandonVel47) | [#151](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/151) |
| Julio Cesar Caballero Perez | [JulioCesarCaballero](https://github.com/JulioCesarCaballero) | [#149](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/149) |

Las boletas y datos de identificación escolar se registran en Classroom, no en este repositorio.

## Resumen de la entrega

| Elemento | Valor |
|---|---|
| Proyecto | [gabrielhuav/PolitecnicoOpenWorld](https://github.com/gabrielhuav/PolitecnicoOpenWorld) |
| Issue | [BrandonVel47/PolitecnicoOpenWorld#2](https://github.com/BrandonVel47/PolitecnicoOpenWorld/issues/2) |
| Pull Request | [#151: feat(sf): add Extraordinary moves (MK-style finishers) and Resit Exam practice mode](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/151) |
| Rama | `BrandonVel47:Movimiento_Final_a_3_Personajes` hacia `gabrielhuav:main` |
| SHA base | `7ed325393f82872c2be94ff2ada46948efa19152` |
| SHA probado | `214a48c9b6515e3a6a63bd46fc32daa275809d3d` (CP-01 a CP-06) |
| SHA final entregado | `c303ce38245f8d15429bae70855762ef7246710e` (corrección H-3, CP-07 repetido) |
| Matriz de pruebas | [docs/pruebas.md](docs/pruebas.md) |
| Evidencias | [evidencias/](evidencias/) |
| Estado del PR | Abierto, listo para revisión. Checks pendientes de aprobación del mantenedor. |

## Objetivo

Agregar al modo de peleas un remate de fin de combate, llamado **Extraordinario**, para tres peleadores, y un modo de práctica, **Examen Extraordinario**, para aprenderlo. El objetivo del examen es someter ese cambio a revisión mediante un PR con QA reproducible.

## Alcance

**Comportamiento actual (versión base):** el juego tiene un `FATALITY`, pero es un súper ataque que se lanza en cualquier momento de la pelea (correr + R2). El golpe que decide el combate siempre termina en un KO clásico y no existe un modo para practicar remates.

**Comportamiento esperado:**

- Al ganar la ronda que decide el combate, el rival queda de pie, mareado y con 0 de vida, y aparece ACABALO durante 5 segundos. Si el ganador mete el comando de su personaje a la distancia correcta, se ejecuta una cinemática.

  | Peleador | Extraordinario | Comando | Distancia |
  |---|---|---|---|
  | La Tzitzimime | NOCHE SIN SOL | ↓ ↓ ← + B | Cerca |
  | La Llorona | EL RÍO TE LLEVA | ← → ← + A | Lejos |
  | El Charro Negro | PACTO COBRADO | → ↓ → + Y | Cerca |

- En el Examen Extraordinario solo se eligen peleadores con Extraordinario, no hay límite de tiempo, una jerga en el piso marca la distancia (roja fuera de rango, verde dentro) y un panel PASOS muestra el comando.

**Usuario afectado:** jugadores del modo de peleas sin conexión (VS CPU, arcade, IA vs IA).

**Fuera del alcance:** partidas en línea (la cinemática no se sincroniza por red), corrección de los datos de la animación TALK en los 18 peleadores (H-1) y del menú con fuente máxima (H-2).

**Criterios de aceptación:** ver la sección 2 de [docs/pruebas.md](docs/pruebas.md).

## Ejecución inicial (versión base)

En la versión base (`7ed3253`), al ganar la ronda que decide el combate el rival cae directamente con KO clásico, sin ventana de remate, y el menú de modos no incluye el Examen Extraordinario. `<Si tienes una captura o video del juego antes de los cambios, enlázalo aquí; si no, deja la siguiente línea.>` No se conservó evidencia en video de la versión base; el comportamiento se verificó al ejecutarla antes de modificar el código.

## Resultados del QA

| ID | Tipo | Estado | Evidencia |
|---|---|---|---|
| CP-01 | Ruta feliz (combate) | Aprobado | [Video 1](evidencias/Video_1.mp4) |
| CP-02 | Ruta feliz (modo práctica) | Aprobado | [Video 2](evidencias/Video_2.mp4), capturas |
| CP-03 | Condición alterna | Aprobado | [Video 3](evidencias/Video_3.mp4) |
| CP-04 | Regresión | Aprobado | [Video 1](evidencias/Video_1.mp4) (ronda 1) |
| CP-05 | Navegación y estado | Aprobado | [Video 2](evidencias/Video_2.mp4) (final) |
| CP-06 | Pruebas automatizadas | Aprobado | 19/19, captura |
| CP-07 | Accesibilidad | Aprobado tras corregir H-3 | Capturas antes y después |
| CP-08 | Compatibilidad | No ejecutado | Justificado en la matriz |

**Hallazgos**

| ID | Descripción | Estado |
|---|---|---|
| H-1 | El Charro Negro se hundía bajo el escenario al iniciar su Extraordinario (origen incorrecto en la animación TALK de todos los peleadores). | Corregido en `214a48c`; el defecto de datos queda como preexistente. |
| H-2 | Con la fuente del sistema al máximo no se puede llegar a Otros modos. | Preexistente, fuera del alcance. |
| H-3 | El botón OCULTAR se cortaba con fuente grande. | Corregido en `c303ce3`; CP-07 repetido. |

El detalle de cada caso (pasos, esperado, real y evidencia) está en [docs/pruebas.md](docs/pruebas.md).

## Verificaciones automáticas (checks)

| Verificación | Estado | Detalle |
|---|---|---|
| PR Quality Gate (`.github/workflows/pr-quality-gate.yml`) | Bloqueado: pendiente de aprobación | El PR viene de un fork y GitHub requiere que el mantenedor autorice la ejecución del workflow. Mensaje en el PR: "1 workflow awaiting approval". |
| Pruebas unitarias (local) | Aprobado | `:shared:testAndroidHostTest`, `SfFinisherTest` 19/19, ejecutado desde Android Studio. |
| detekt (local) | Aprobado | Configuración del proyecto (`config/detekt/detekt.yml` con su baseline), sin hallazgos nuevos. |
| Compilación debug (local) | Aprobado | La app compiló e instaló desde Android Studio en el dispositivo de prueba. |

**Qué cubre el workflow:** construcción debug de Android, pruebas unitarias de `app` y `shared`, comprobación de nombres de pruebas KMP y análisis estático con detekt. **Qué no cubre:** el comportamiento en un dispositivo real, la parte visual y la accesibilidad; eso se validó con los casos manuales.

**Limitación local:** `gradlew.bat` no se pudo ejecutar porque falta `gradle-wrapper.jar` en la copia del repositorio; las mismas tareas se ejecutaron desde el Gradle de Android Studio. Este bloqueo se documenta como externo y no se presenta como prueba aprobada del workflow.

## Revisión

| Rol | Persona | Enlace | Estado |
|---|---|---|---|
| Revisión recibida en mi PR | `<usuario>` | `<liga al comentario de revisión>` | Pendiente |
| Revisión que hice a otro PR | #149 (JulioCesarCaballero) | https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/149#pullrequestreview-5375785108 | Realizada |

`<Al terminar: resumen de las observaciones recibidas, cómo se respondió cada una y si hubo commits nuevos.>`

## Conclusiones

El cambio cumple los tres criterios de aceptación en el dispositivo de prueba: el Extraordinario se ejecuta correctamente, los casos alternos terminan en KO clásico o REPROBADO, y el modo de práctica funciona sin límite de tiempo. Durante el QA se encontraron tres defectos; dos eran del cambio y se corrigieron con commits nuevos (H-1, H-3), y uno es preexistente del juego (H-2). Se recomienda integrar el cambio, sujeto a que el PR Quality Gate pase cuando el mantenedor autorice su ejecución.

Riesgos que permanecen: compatibilidad no verificada en otro idioma o dispositivo, TalkBack no probado y los Extraordinarios deshabilitados a propósito en partidas en línea.

## Bitácora: BrandonVel47

**Commits en el PR #151**

| SHA | Mensaje |
|---|---|
| `2f4c724` | Agrega Remate Final estilo MK para Tzitzimime, Llorona y Charro Negro |
| `cd6b72131209b715c67e4f0f7b41fa2f7e32b523` | feat(sf): add Resit Exam practice logic for Extraordinary moves |
| `f80d19f06f565cdfddeb01da9a20c387fb326d9c` | feat(sf): add Resit Exam menu, steps panel and pixel-art jerga |
| `214a48c` | fix(sf): stop Charro Negro sinking into the floor at the start of his Extraordinary |
| `c303ce3` | fix(sf): let Resit Exam buttons grow with large system font |

**Casos ejecutados:** CP-01 a CP-07 (todos por BrandonVel47).

**Revisión realizada:** [#149 (JulioCesarCaballero)](https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/149#pullrequestreview-5375785108)

## Uso de herramientas de IA

| Herramienta | Propósito |
|---|---|
| Claude (Anthropic) | Análisis del código del modo de peleas, propuesta de diseño, redacción del código del Extraordinario, del modo Examen Extraordinario y de sus pruebas unitarias, diagnóstico de los hallazgos H-1 y H-3, y borradores de la descripción del PR y de este documento. |

**Trabajo propio:** definición de la idea y del alcance (nombres "Extraordinarios" y "Examen Extraordinario", uso de la jerga, personajes elegidos), integración de los cambios en el repositorio, compilación e instalación en el dispositivo, ejecución de todos los casos de prueba, grabación de las evidencias, detección de los hallazgos H-1, H-2 y H-3 durante las pruebas, y gestión del fork, la rama, el issue y el PR. Todas las evidencias de ejecución son reales y corresponden a los SHA indicados.

## Referencias

- Instrucciones del examen: Primer examen parcial, Pull Request con aseguramiento de calidad, 7CV4, 2027-1.
- README del proyecto POW y `.github/workflows/pr-quality-gate.yml` del repositorio original.
- Guías del proyecto en `README for IAS/` (convenciones, configuración del entorno).