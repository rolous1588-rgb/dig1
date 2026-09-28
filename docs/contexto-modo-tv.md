# Contexto: material interactivo "modo TV" para Electrónica Digital I

Resumen del trabajo hecho con Claude (chat) entre el 24 y el 28 de septiembre de 2026. Sirve como punto de partida para seguir desarrollando el material de clase en Claude Code.

---

## 1. Quién y para qué

- **Profesor:** Rolando Santillan, UMRPSFXCH (Sucre, Bolivia), Facultad de Ingeniería y Ciencias Aplicadas Mecaelectrónicas.
- **Materia:** Electrónica Digital I. Grupos mixtos de ingeniería electrónica y mecatrónica.
- **Libro de referencia:** Tocci, *Sistemas Digitales* (edición en español, 13 capítulos).
- **Realidad del aula:** los estudiantes llegan con nivel bajo desde el colegio. El enfoque es **primeros principios** y contenido aplicable a la vida real, no cubrir todo el libro.
- **Objetivo del material:** clases presenciales dinámicas. El profesor proyecta diagramas interactivos desde su celular al televisor, los estudiantes trabajan sin celulares y con hojas de trabajo impresas ligadas a lo proyectado.

---

## 2. Temario acordado para Digital 1 (sin HDL)

Orden de primeros principios: representación, álgebra, diseño combinacional, bloques estándar, aritmética y memoria. El HDL queda para Digital 2.

**Antes del 1er parcial**
- Cap. 1: 1-1 a 1-5 (1-6 a 1-8 como lectura).
- Cap. 2: 2-1 a 2-5, 2-7, 2-9 (ASCII de pasada).
- Cap. 3: 3-1 a 3-14.
- Cap. 4: 4-1 a 4-9. Las secciones 4-10 a 4-13 (diagnóstico de fallas) se usan como guía de laboratorio.

**Entre parciales**
- Cap. 9, bloques MSI, adelantado porque es combinacional puro: 9-1, 9-2, 9-4, 9-6 a 9-8, 9-10.
- Cap. 6, aritmética: 6-1 a 6-4, 6-9 a 6-11, 6-14.

**Después del 2do parcial**
- Cap. 5, flip-flops: 5-1, 5-2, 5-4 a 5-10, 5-19, 5-21, 5-23.

**Fuera de Digital 1:** Cap. 7, ALU, Cap. 10, 12 y 13 pasan a Digital 2. El Cap. 8 se ve solo en lo práctico de 4-9. El Cap. 11 queda para Digital 2 o microcontroladores.

---

## 3. Cómo se proyecta (condiciones reales, medidas en el aula)

- **Televisor:** 74–75", institucional, **sin internet**. No se usa su navegador.
- **Transmisión:** un dispositivo instalado en el TV, con QR, **duplica la pantalla del celular**. La página se dibuja con las medidas del celular, no con las del televisor.
- **Celular del profesor:** Poco X8 Pro, Android, pantalla de 2756 × 1268.
- **Cómo abre el material:** descarga un **archivo HTML** y lo abre con **Chrome** desde Files de Google (queda como `content://...`). Prefiere esto a abrir enlaces.
- **Distancia a la última fila:** 7,2 m.
- **Resultado de la medición** (25/09/2026):

| Dato | Valor |
|---|---|
| Espacio útil con pantalla completa | **848 × 390 px** |
| Espacio útil sin pantalla completa | 800 × 270 px (inservible) |
| Densidad de píxeles | 3,25 |
| Imagen en el TV | Completa, sin recorte ni estiramiento, con franjas negras arriba y abajo |
| Pantalla completa en Chrome | Funciona |
| Letra mínima legible desde el fondo | **28 px** (comprobado; coincide con la regla AVIXA de distancia ÷ 200 ≈ 3,6 cm) |
| Obstáculo físico | Una etiqueta pegada en la esquina superior derecha del TV tapa un poco esa esquina |

---

## 4. Especificación técnica del "modo TV"

1. **Un solo archivo HTML autocontenido.** Sin fuentes de Google ni librerías externas; usar fuentes del sistema (Roboto en Android) y SVG en línea. Debe funcionar sin internet, porque el dispositivo del QR puede dejar al celular sin conexión.
2. **Lienzo fijo de 848 × 390 px**, centrado y escalado con `transform: scale()` para caber en cualquier pantalla. En el celular, en pantalla completa, la escala es 1. Fuera del lienzo, fondo negro.
3. **Pantalla completa obligatoria.** Una pantalla de inicio con botón "Empezar" hace `requestFullscreen({navigationUI:'hide'})` y `screen.orientation.lock('landscape')`, ambos con try/catch. Si el usuario sale de pantalla completa, aparece un botón pequeño "Volver a pantalla completa". En vertical, un mensaje pide girar el celular.
4. **Sin desplazamiento.** Todo cabe en 848 × 390. Caben unas 10 líneas de 28 px.
5. **Letra mínima de 28 px** para todo lo que leen los estudiantes, incluidas las etiquetas de los diagramas. El texto que solo lee el profesor puede ser más chico (15–20 px): contadores, "1 de 3", etiquetas de botones.
6. **Nada importante en la esquina superior derecha.**
7. **Botones de al menos 48 px de alto.** El pulgar del deslizador, de unos 40 px.
8. **Lecciones técnicas de los prototipos:**
   - Usar `overflow: clip` en el contenedor de la galería y forzar `scrollLeft = 0`. El foco en botones de pantallas ocultas desplazaba el contenedor y descuadraba la galería.
   - En grids, usar `grid-template-rows: minmax(0,1fr)`. En flex, usar `min-height: 0` y `flex: 1 1 0`; sin eso el contenido se salía por abajo al agregar la barra de pestañas.
   - El gesto de deslizar ignora los toques que empiezan sobre `button` o `input`, para que los deslizadores y botones funcionen.
   - Probar siempre a 848 × 390 antes de entregar, con capturas de cada pantalla.

---

## 5. Decisiones de experiencia de usuario (validadas en el aula)

- **Navegación:** galería, pasando de pantalla deslizando el dedo (también con las flechas del teclado). Las pestañas se probaron como alternativa y quitan unos 46 px de alto.
- **Ritmo por concepto: Predecir → Observar → Explicar**, tres pantallas seguidas:
  1. **Predecir** (fondo tipo pizarra): solo la pregunta, en letra grande (~40 px), y opciones grandes. El profesor cuenta las manos levantadas y toca cada opción una vez por mano; hay un enlace "Borrar votos".
  2. **Observar** (fondo claro): el diagrama interactivo. A la derecha, una franja **"Sus pruebas"**, una tabla que se llena sola con cada prueba (al soltar el deslizador), ordenada por valor y con color por resultado. Además, puntos de color sobre la escala del diagrama.
  3. **Explicar** (fondo pizarra): "Lo que predijeron" (barras de votos, con la respuesta correcta marcada) frente a "Lo que midieron" (resumen calculado de sus propios datos). La **idea clave** se revela recién al tocar, después de la discusión.
- **Se descartó la pizarra de preguntas fija al costado.** Ocupaba mucho espacio y distraía del diagrama. Las preguntas viven en su propia pantalla.
- **La franja lateral solo aparece si ayuda a entender.** Opciones: tabla que se llena sola (la preferida), fórmula con los valores del momento, vista real del componente (chip, protoboard, osciloscopio), reto con objetivo, o comparación antes y después.
- **Control según la naturaleza de la variable:**
  - deslizador para magnitudes continuas (voltaje, ruido, tiempo);
  - botones − + para pasos discretos donde cada paso importa (bits, cantidad de muestras);
  - tocar el diagrama cuando el elemento mismo es el control (LEDs, bits de un diagrama de tiempo).
- **Contenido:** una idea por pantalla y preguntas de 90 caracteres como máximo (unas 15 palabras).
- **Colores:** papel claro (#EEF2F0), tinta verde muy oscura (#14201B), azul para lo digital (#1C4DB3), cobre para lo analógico (#B0531A), rojo LED para el 1, ámbar con rayado para la zona indefinida, y pizarra verde oscura (#1B2A25) con tiza amarilla (#F2C94C) para preguntas y conclusiones. **Nota:** el profesor dijo en otro contexto que no le gustan los colores oscuros en su plataforma. Las pantallas de pizarra oscura se aceptaron en la prueba, pero conviene confirmarlo antes de generalizarlas.

---

## 6. Qué se probó y se descartó (y por qué)

| Idea | Resultado |
|---|---|
| Página diseñada para laptop o proyector (≥1280 × 720), con barra de pestañas y panel lateral de preguntas | Proyectada desde el celular se cortaba: el celular solo ofrece 848 × 390 |
| Que el TV abra la página en su navegador y el celular sirva de control remoto (Supabase Realtime) | Descartado: el TV no tiene internet y es institucional |
| Instalarla como app (PWA) desde GitHub Pages | Innecesario: el profesor prefiere descargar el HTML |
| Panel de preguntas permanente a la derecha | Descartado: ocupa espacio y distrae |
| Solo deslizador o solo botones − + | Depende del caso (ver la regla del control según la variable) |

---

## 7. Pedagogía y plan de prueba en el aula

- **Celulares fuera:** los estudiantes guardan el celular en la mochila, lejos. Evidencia: con dispositivos permitidos, los exámenes bajaron cerca de un 5 %, también para quienes no los usaban (Glass y Kang, 2019). Hay que explicar el porqué, que el profesor dé el ejemplo (su celular solo es el control remoto) y cuidar que ningún ejercicio necesite calculadora del celular.
- **Hojas de trabajo "espejo":** son apuntes guiados, con efecto moderado según un metaanálisis (d ≈ 0,55; Larwin y otros, 2013). Cada pantalla tiene su recuadro en la hoja, con el mismo nombre.
  - **Predecir:** escriben su predicción **antes** de votar.
  - **Observar:** una tabla con las mismas columnas que "Sus pruebas".
  - **Explicar:** una frase para completar; la idea clave la escriben ellos.
  - **Ejercicios con ayuda que se retira:** uno resuelto en el TV, uno a medio resolver y uno solo.
  - **Problemas solos:** 2–3, de dificultad creciente, al menos uno de la vida real.
  - **Boleto de salida:** una tira con 2 preguntas que cortan y entregan. La hoja se va a casa y la tira se queda con el profesor.
  - **Formato:** tamaño carta, una hoja por ambos lados. **Que funcione en fotocopia blanco y negro** (símbolos 0 / ? / 1 y rayados, no depender del color). Espacio amplio para escribir a mano. Encabezado con nombre, fecha y número de hoja. Recuadros siempre en el mismo lugar, para poder corregirlos con IA más adelante.
- **Cómo medir cada prueba** (2 minutos al terminar la clase):
  1. cuántos completaron la hoja en clase;
  2. cuántos respondieron bien el boleto de salida;
  3. a mitad de la clase, cuántos estaban trabajando o mirando la pantalla.

  Si se puede, comparar con una clase dada de la forma habitual.

---

## 8. Forma de trabajo acordada

- **Probar en pequeño, someterlo a la realidad y mejorar.** Nada grande de una sola vez.
- **Conversar las decisiones de formato antes de generar material.**
- Entregar **archivos HTML** para descargar; el profesor los usa desde el celular.
- Antes de entregar, revisar con capturas a 848 × 390.

---

## 9. Contenido de la Clase 1 (Cap. 1, 90 min), para rehacer en modo TV

La primera versión (`ceros-y-unos.html`) se diseñó para laptop y no sirve proyectada. Hay que rehacerla con el ritmo Predecir → Observar → Explicar. Los conceptos y sus piezas:

| Concepto (Tocci) | Min | Pregunta de predicción | Diagrama y control | Idea clave |
|---|---|---|---|---|
| Motivación | 0–10 | ¿La temperatura o la voz cambian de forma continua o a saltos? | Cadena sensor → ADC → procesador → DAC → actuador; tocar bloques; ejemplos: termostato, celular, brazo robótico | El mundo es analógico y lo procesamos en digital, entre el ADC y el DAC |
| 1-1/1-2 Digitalizar | 10–18 | Con 1 bit, ¿qué queda de la temperatura? | Curva de temperatura de 0 a 32 °C en 24 h, con botones − + de bits (1–8); resolución = 32 ÷ 2ᴺ | Digitalizar es muestrear y cuantizar; N bits dan 2ᴺ niveles |
| 1-2 Ruido | 18–25 | Si subo el ruido, ¿cuál señal se daña primero? | Señal analógica y digital (16 bits) con deslizador de ruido; umbral de 2,5 V; bits enviados frente a recibidos | La señal digital se regenera mientras el ruido no cruce el umbral |
| 1-3 Binario | 25–50 | ¿Cuál es el número más grande con 4 bits? | 4 LEDs 8-4-2-1 que se tocan; +1 y conteo automático; retos ("formen el 13") | Cada posición vale el doble; con N bits hay valores de 0 a 2ᴺ − 1 |
| 1-4/1-5 Niveles lógicos | 50–70 | Con 1,5 V, ¿la compuerta TTL lee 0 o 1? | **Ya hecho:** `prueba-niveles-poe.html`. TTL: VIL = 0,8 V y VIH = 2,0 V. Posible extensión: CMOS 74HC a 5 V (1,5 V / 3,5 V) y entrada al aire | La compuerta no mide: decide |
| 1-4 Diagramas de tiempo | 70–85 | Si cada bit dura 1 ms, ¿cuánto tarda un byte? | Forma de onda de 8 bits; modos leer y escribir tocando; marcar flancos | Nivel alto = 1, nivel bajo = 0, cada bit ocupa un intervalo fijo |
| Cierre | 85–90 | ¿Cómo escribirían 45 en binario? (puente al Cap. 2) | Tarea: 1011, 11001, 100000, 1111111, 10101010 → 11, 25, 32, 127, 170 | — |

---

## 10. Archivos generados hasta ahora

| Archivo | Estado |
|---|---|
| `prueba-niveles-poe.html` | **Referencia validada.** Implementa el patrón Predecir → Observar → Explicar con un concepto |
| `prueba-modo-tv.html` | Prototipo anterior: galería o pestañas, panel de preguntas fijo (descartado), deslizador frente a botones − + |
| `medidor-pantalla.html` | Herramienta para medir el espacio útil en otra aula o con otro celular |
| `ceros-y-unos.html` / `digital1-cap1.html` | Clase 1 original, para laptop. Sirve solo como fuente de contenido |

---

## 11. Próximos pasos sugeridos (en orden, cada uno pequeño)

1. **Hoja de trabajo de niveles lógicos**, en PDF tamaño carta, espejo de `prueba-niveles-poe.html`. Probarla en clase y medir.
2. **Plantilla reutilizable:** separar el motor (lienzo, galería, pantalla completa, pantallas Predecir/Observar/Explicar) del contenido, para que cada concepto nuevo sea casi solo datos y un diagrama.
3. **Rehacer la Clase 1** concepto por concepto con la plantilla, probando uno o dos por clase.
4. Más adelante: publicar los materiales en el sitio `sitio-asignaturas` (GitHub Pages).

## 12. Decisiones abiertas

- ¿Se mantienen las pantallas de pizarra oscura, o todo en claro?
- ¿Hace falta un botón para restar votos en la pantalla Predecir?
- ¿Cuántos conceptos caben por clase de 90 minutos con hoja de trabajo? Lo dirá la primera prueba.
