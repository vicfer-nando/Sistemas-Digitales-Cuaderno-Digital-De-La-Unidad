# Sistemas Digitales · Unidad 1

**Microcontroladores para sistemas embebidos**

| Dato académico | Información |
|---|---|
| Universidad | Universidad Nacional de Loja |
| Carrera | Computación |
| Asignatura | Sistemas Digitales |
| Docente | Ing. Luis D. Sinche |
| Estudiante | Victor Fernando Roa Carrión |
| Período académico | Septiembre 2026 – Marzo 2027 |

## Índice general

| Semana | Contenido | Índice |
|---|---|---|
| 1 | Microcontroladores, CPU, buses, memoria, bases numéricas, bits y AVR | [Fundamentos](Semana_01_Fundamentos/README.md) |
| 2 | ESP32 clásico, placa, GPIO, arranque, tipos de datos, LED y pulsadores | [ESP32](Semana_02_ESP32/README.md) |
| 3 | Contadores, prescaler, alarmas, autorrecarga, API y ejercicios | [Temporizadores](Semana_03_Temporizadores/README.md) |
| 4 | Interrupciones GPIO, ISR, IRAM, variables compartidas y antirrebote | [Interrupciones externas](Semana_04_Interrupciones/README.md) |

## Cómo estudiar con este cuaderno

Conviene seguir el orden de cada índice: primero entender el hardware, después leer el ejemplo y finalmente resolver la pregunta de comprobación. Los repasos semanales reúnen las ideas que deberían poder explicarse sin mirar el texto.

Los ejemplos AVR corresponden al **ATmega328P de un Arduino Uno clásico**. Los ejemplos ESP32 corresponden al **ESP32 original con módulo WROOM**, habitual en DevKit V1. No deben trasladarse automáticamente a ESP32-C3, S3 u otras variantes. En temporizadores se usa **Arduino-ESP32 3.x**; las llamadas 2.x se muestran aparte para reconocer material antiguo.

Los bloques marcados como fragmentos se insertan en el contexto indicado. Los ejemplos con `setup()` y `loop()` son sketches didácticos independientes: no se concatenan entre sí. Los valores calculados describen el modelo teórico; no representan mediciones de laboratorio.
