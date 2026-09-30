# Plan y matriz de pruebas: Extraordinarios y Examen Extraordinario

PR: https://github.com/gabrielhuav/PolitecnicoOpenWorld/pull/151
Issue: `BrandonVel47/PolitecnicoOpenWorld#<N>`
Rama: `Movimiento_Final_a_3_Personajes`
SHA base: `<SHA base>`
SHA probado: `<SHA de tu último commit>`
Versión de la app: 1.0.0.18 (build debug de la rama)

> Cómo llenar este documento: el plan (pasos y resultado esperado) ya está escrito. Solo se
> llenan **Resultado real**, **Estado**, **Fecha** y **Evidencia** DESPUÉS de ejecutar cada caso.
> Estados válidos: Aprobado, Fallido, Bloqueado, No aplica.

## 1. Entorno

| Campo | Valor |
|---|---|
| Sistema operativo | Windows `<versión>` |
| Android Studio | `<versión, en Help > About>` |
| JDK | JBR 21 incluido en Android Studio |
| SDK | compileSdk 36, minSdk 24 |
| Dispositivo principal | Samsung SM-A566E, Android 16 (API 36) |
| Dispositivo secundario | `<emulador AVD, modelo y API, si se usa en CP-08>` |
| Datos de prueba | Ninguno personal. Sin `google-services.json`; `MAPS_API_KEY=DEFAULT_API_KEY` |

## 2. Criterios de aceptación

| ID | Criterio |
|---|---|
| CA-1 | Éxito: al ganar la ronda que decide el combate y meter el comando correcto a la distancia correcta, se ejecuta el Extraordinario y termina el combate. |
| CA-2 | Alterna: si la ventana ACABALO vence, el comando es incorrecto o el ganador golpea normal, el combate termina con KO clásico. |
| CA-3 | Modo práctica: solo se pueden elegir peleadores con Extraordinario, no hay límite de tiempo y la jerga se pone verde solo dentro del rango válido. |

## 3. Riesgos

| ID | Riesgo | Impacto | Caso que lo cubre |
|---|---|---|---|
| R-1 | El gancho en `endRound` rompe el fin de ronda normal de peleadores sin Extraordinario. | Alto: afectaría todas las peleas. | CP-05 |
| R-2 | El comando no funciona o las flechas se muestran mal cuando el jugador está a la derecha del rival. | Medio: el movimiento sería imposible desde un lado. | CP-09 |
| R-3 | En el Examen Extraordinario el reloj corre o el combate termina, en lugar de reiniciar el intento. | Medio: el modo no serviría para practicar. | CP-02, CP-04 |
| R-4 | Con texto grande, el panel PASOS o los botones se cortan o tapan los controles. | Bajo/medio: problema de accesibilidad. | CP-07 |
| R-5 | Faltan textos en inglés y la pantalla muestra claves o texto vacío. | Bajo: problema visual en otro idioma. | CP-08 |

## 4. Resumen de ejecución

| ID | Tipo | Cubre | Estado |
|---|---|---|---|
| CP-01 | Ruta feliz | CA-1 | |
| CP-02 | Ruta feliz (modo práctica) | CA-3, R-3 | |
| CP-03 | Condición alterna | CA-2 | |
| CP-04 | Condición alterna / repetición | CA-2, R-3 | |
| CP-05 | Regresión | R-1 | |
| CP-06 | Navegación y estado | CA-3 | |
| CP-07 | Accesibilidad | R-4 | |
| CP-08 | Compatibilidad (idioma) | R-5 | |
| CP-09 | Límite (lado opuesto) | R-2 | |
| CP-10 | Pruebas automatizadas | CA-1, CA-3 | |

## 5. Fichas de casos

### CP-01: Extraordinario en combate normal

- **Cubre:** CA-1
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** App instalada desde la rama. Modo Práctica (VS CPU), dificultad Básica.
- **Datos:** Peleador: El Charro Negro. Rival: cualquiera.
- **Pasos:**
  1. Menú de peleas, Otros modos, Práctica.
  2. Elegir El Charro Negro, un rival, dificultad Básica y un escenario.
  3. Ganar la ronda 1.
  4. En la ronda 2, dejar la vida del rival en 0.
  5. Cuando aparezca ACABALO, acercarse al rival y meter → ↓ → + Y.
- **Resultado esperado:** El rival queda de pie y mareado con 0 de vida; aparece ACABALO; al meter el comando se oscurece la escena, el rival arde y desaparece; aparece EXTRAORDINARIO y PACTO COBRADO; después sale el menú de fin de pelea con el Charro como ganador.
- **Resultado real:**
- **Estado:**
- **Evidencia:** `<liga a video o capturas>`
- **Defecto asociado y decisión:**

### CP-02: Aprobar en el Examen Extraordinario

- **Cubre:** CA-3, R-3
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** App instalada desde la rama.
- **Datos:** Peleador: La Tzitzimime.
- **Pasos:**
  1. Menú de peleas, Otros modos, EXAMEN EXTRAORDINARIO.
  2. Verificar que en el selector solo aparezcan La Tzitzimime, La Llorona y El Charro Negro.
  3. Elegir La Tzitzimime.
  4. Esperar 30 segundos sin hacer nada.
  5. Caminar hacia el rival hasta que la jerga del piso se ponga verde.
  6. Presionar PASOS y meter ↓ ↓ ← + B, verificando que cada paso se palomee.
- **Resultado esperado:** Solo aparecen los 3 peleadores. La pelea empieza directo en ACABALO, sin conteo de ronda. Tras 30 segundos no pasa nada (el reloj no corre y la ventana no vence). La jerga pasa de roja a verde al entrar en rango. Se ejecuta NOCHE SIN SOL, aparece APROBADO y el intento se reinicia solo con el rival de nuevo mareado.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-03: La ventana ACABALO vence

- **Cubre:** CA-2
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** Igual que CP-01.
- **Datos:** Peleador: La Llorona.
- **Pasos:**
  1. Repetir los pasos 1 a 4 de CP-01 con La Llorona.
  2. Cuando aparezca ACABALO, no tocar nada durante 6 segundos.
- **Resultado esperado:** A los 5 segundos el rival cae (KO clásico), se quita el velo oscuro y aparece el menú de fin de pelea normal. No se ejecuta ningún Extraordinario.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-04: Reprobar con un golpe normal y repetir

- **Cubre:** CA-2, R-3
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** Dentro del Examen Extraordinario.
- **Datos:** Peleador: El Charro Negro.
- **Pasos:**
  1. Entrar al Examen Extraordinario con El Charro Negro.
  2. Acercarse al rival y golpearlo con un puño normal (sin comando).
  3. Esperar a que se reinicie.
  4. Repetir los pasos 2 y 3 dos veces más.
- **Resultado esperado:** Cada golpe normal tumba al rival y muestra REPROBADO. A los 1.5 segundos el intento se reinicia, sin menú de fin de pelea. Las tres repeticiones se comportan igual y el reloj nunca avanza.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-05: Regresión con peleador sin Extraordinario

- **Cubre:** R-1
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** Modo Práctica (VS CPU).
- **Datos:** Peleador: Escomboy (no tiene Extraordinario).
- **Pasos:**
  1. Jugar un combate con Escomboy y ganar 2 rondas por KO.
  2. Jugar otro combate y dejar que una ronda termine por tiempo.
- **Resultado esperado:** Al ganar la ronda decisiva, el rival cae directamente con KO clásico: no aparece ACABALO ni la jerga. La ronda por tiempo termina normal. Todo se comporta igual que en `main`.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-06: Navegación y ciclo de vida

- **Cubre:** CA-3
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** Dentro del Examen Extraordinario.
- **Datos:** Cualquier peleador con Extraordinario.
- **Pasos:**
  1. Presionar SALIR.
  2. Verificar a qué pantalla regresa y volver a entrar con otro peleador.
  3. Durante ACABALO, presionar el botón Inicio del teléfono y volver a abrir la app.
  4. Presionar la X de arriba a la derecha.
  5. Intentar girar el teléfono.
- **Resultado esperado:** SALIR regresa al selector del Examen Extraordinario (no al de Arcade). Al volver de segundo plano la pelea sigue en el mismo estado (rival mareado, sin terminar). Registrar a dónde lleva la X. Registrar si la orientación está fija en horizontal.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-07: Accesibilidad (texto grande y controles)

- **Cubre:** R-4
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** En Ajustes del teléfono, Pantalla, Tamaño de fuente al máximo.
- **Datos:** Peleador: La Tzitzimime.
- **Pasos:**
  1. Abrir el Examen Extraordinario y presionar PASOS.
  2. Revisar que el panel, los textos y los botones PASOS y SALIR se lean completos y no tapen el joystick ni los botones.
  3. Revisar que PASOS y SALIR se puedan tocar con facilidad.
  4. Regresar el tamaño de fuente a normal.
- **Resultado esperado:** Los textos se leen completos y los botones son fáciles de tocar. Limitación a registrar: la jerga indica el rango solo con color (rojo/verde); como apoyo, el panel PASOS también muestra la distancia en texto ("Distancia: CERCA"). Los textos dibujados dentro de la escena (ACABALO) usan la fuente del juego y no cambian con el tamaño de fuente del sistema.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-08: Compatibilidad (idioma inglés)

- **Cubre:** R-5
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** `<SM-A566E o emulador>`
- **Precondiciones:** Idioma del teléfono (o de la app) en inglés.
- **Datos:** Peleador: La Llorona.
- **Pasos:**
  1. Abrir el menú de modos.
  2. Entrar al modo y presionar STEPS.
  3. Ejecutar el Extraordinario desde lejos (jerga verde).
- **Resultado esperado:** El botón dice RESIT EXAM; el panel dice STEPS/HIDE y "Range: FAR"; en la escena sale FINISH HIM o FINISH HER (según el rival) y después EXTRAORDINARY y THE RIVER TAKES YOU; al terminar sale PASSED!. Ningún texto aparece vacío ni como clave interna.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-09: Jugador del lado derecho

- **Cubre:** R-2
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:** | **Dispositivo/API:** SM-A566E / 36
- **Precondiciones:** Dentro del Examen Extraordinario.
- **Datos:** Peleador: El Charro Negro.
- **Pasos:**
  1. Saltar por encima del rival para quedar a su derecha.
  2. Presionar PASOS.
  3. Acercarse hasta que la jerga (ahora del lado derecho) se ponga verde.
  4. Meter el comando siguiendo las flechas del panel.
- **Resultado esperado:** La jerga aparece del lado derecho del rival. El panel muestra las flechas volteadas: ← ↓ ← + Y. Al meterlo así se ejecuta PACTO COBRADO y sale APROBADO.
- **Resultado real:**
- **Estado:**
- **Evidencia:**
- **Defecto asociado y decisión:**

### CP-10: Pruebas automatizadas

- **Cubre:** CA-1, CA-3 (lógica aislada)
- **Autor:** BrandonVel47 | **Fecha:** | **SHA:**
- **Precondiciones:** Proyecto sincronizado en Android Studio.
- **Pasos:**
  1. Abrir `shared/src/commonTest/.../streetfighter/SfFinisherTest.kt`.
  2. Presionar el botón de ejecutar junto a `class SfFinisherTest` y elegir Run.
- **Resultado esperado:** 19 tests aprobados (lectura de comandos, rangos, catálogo, efectos, flechas volteadas, zona de la jerga y progreso de pasos).
- **Resultado real:** `../evidencias/CP-10_tests_19_passed.png`
- **Estado:**
- **Evidencia:**
- **Nota:** `gradlew.bat` no se pudo ejecutar localmente porque falta `gradle-wrapper.jar` en la copia del repositorio (limitación del entorno, no del cambio). Se ejecutó la misma tarea `:shared:testAndroidHostTest` desde Android Studio. Los checks del PR quedan pendientes de aprobación del mantenedor.

## 6. Hallazgos

| ID | Descripción | Pasos | Esperado vs observado | Severidad | Estado |
|---|---|---|---|---|---|
| H-1 | En el Extraordinario del Charro Negro el peleador se hunde bajo el escenario durante el primer segundo y luego vuelve a subir. | CP-01 o CP-02 con El Charro Negro: meter el comando y observar el inicio de la cinemática. | Esperado: el Charro se mantiene sobre el piso. Observado: se dibuja 96 px más abajo. Causa: los cuadros de TALK de los 18 peleadores tienen el origen en y=128 en lugar de y=224 (defecto de datos preexistente). | Baja (solo visual) | Corregido: la cinemática usa TAUNT en lugar de TALK y un test impide volver a usar TALK. El defecto de datos de TALK queda como preexistente. |

Avisos preexistentes (también aparecen en `main`, no son del cambio): advertencias `AGPBI ... is deprecated` al compilar y el mensaje `google-services.json NO encontrado`.

## 7. Cierre del QA

- **Recomendación de integración:**
- **Evidencia que la respalda:**
- **Riesgos que permanecen:** los Extraordinarios no funcionan en partidas en línea (excluidos a propósito); el patrón de la jerga se basa en una foto de referencia de internet.
