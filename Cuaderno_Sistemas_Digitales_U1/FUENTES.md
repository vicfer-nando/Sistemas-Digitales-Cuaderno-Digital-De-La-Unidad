# Fuentes utilizadas y trazabilidad

[Índice general](README.md) · [Comparación y decisiones](ANALISIS_DE_FUENTES.md)

## Criterio de uso

Se utilizaron los archivos extraídos **localmente**; no se clonaron los repositorios. Los nombres de autor indicados a continuación aparecen en los documentos o en su README. No se asignan autores a partir del nombre de usuario de GitHub ni se inventan fechas de elaboración.

Las páginas citadas son las posiciones físicas del PDF, empezando por 1. Cuando el documento lleva la misma numeración impresa, ambas coinciden. Los archivos originales se mantienen en su carpeta. Los enlaces a fuentes locales son relativos al espacio de trabajo y seguirán funcionando mientras se conserve esa estructura.

El cuaderno reformula las explicaciones y desarrolla de nuevo los cálculos. Todos los SVG del cuaderno son originales; ninguna fotografía ajena se presenta como propia. Los ejemplos de código se reescribieron para aclarar dispositivo, versión y comunicación entre ISR y programa principal.

## 1. Resumen general

**Clave R:** [Resumen_de_Sistemas_Digitales.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/Resumen_de_Sistemas_Digitales.pdf), 22 páginas. La portada identifica a **Richard Ariel Chamba Santin**.

| Páginas físicas utilizadas | Aporte |
|---|---|
| 2–4 | Microcontrolador, bloques, buses, CPU y reloj |
| 5–6 | Memorias, capacidad, plataformas y arreglo de 2000 int |
| 7–9 | Bases numéricas, máscaras y ensamblador AVR |
| 10 | DDRB, PORTB, PINB y correcciones de escritura |
| 11–14 | ESP32, alimentación, W1TS/W1TC, arranque, tipos y antirrebote |
| 15–17 | Temporizadores, resolución, alarma, autorrecarga y variables compartidas |
| 18–19 | Interrupciones GPIO, ISR y patrón bandera |
| 20–21 | Formulario y ejercicios numéricos rehechos en la semana 3 |

Las pp. 1 y 22 se revisaron para identificar estructura, procedencia y declaración de asistencia; no se usaron como contenido técnico. Las presentaciones que R menciona no se consideran consultadas directamente.

## 2. Carpeta SistemasDigitales_Apuntes-main

Procedencia declarada por el proyecto: repositorio [Kthiu666/SistemasDigitales_Apuntes](https://github.com/Kthiu666/SistemasDigitales_Apuntes). Se trabajó con la extracción local. Los PDF identifican a **Katheryn Contento**.

| Clave | Archivo local realmente consultado | Páginas | Aporte y destino |
|---|---|---|---|
| K01 | [Clase_01_Microcontroladores_y_sistemas_embebidos.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_01_Microcontroladores_y_sistemas_embebidos.pdf) | 3 | Microcontroladores, embebidos y apoyo electrónico; semana 1 |
| K02 | [Clase_02_CPU_buses_y_reloj.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_02_CPU_buses_y_reloj.pdf) | 2 | CPU, PC, buses y período de reloj; semana 1 |
| K03 | [Clase_03_Arquitecturas_memorias_y_bits.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_03_Arquitecturas_memorias_y_bits.pdf) | 4 | Harvard/Von Neumann, memorias, bases, bits e instrucciones; semana 1 |
| K04 | [Clase_04_Entradas_y_salidas_en_AVR.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_04_Entradas_y_salidas_en_AVR.pdf) | 2 | DDRB, PORTB, PINB, entradas y pull-up; semana 1 |
| K05 | [Clase_05_ESP32_registros_y_periféricos.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_05_ESP32_registros_y_perif%C3%A9ricos.pdf) | 3 | Chip/módulo/placa, voltajes, GPIO y W1TS/W1TC; semana 2 |
| K06 | [Clase_06_Arranque_tipos_de_datos_y_pulsadores.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_06_Arranque_tipos_de_datos_y_pulsadores.pdf) | 2 | Arranque, signo, uint8_t, pulsador y estabilidad de 30 ms; semana 2 |
| K07 | [Clase_07_Temporizadores_de_hardware.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_07_Temporizadores_de_hardware.pdf) | 2 | Fórmulas, ejercicios, corrección de 10 kHz y autorrecarga; semana 3 |
| K08 | [Clase_08_Interrupciones_externas.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/SistemasDigitales_Apuntes-main/Clase_08_Interrupciones_externas.pdf) | 2 | Sondeo, flancos, ISR, volatile, bandera y filtro temporal; semana 4 |

K01–K05 incluyen explicaciones o capturas que requieren inspección visual; el texto extraído por sí solo es incompleto. Se contrastaron los manuscritos y las aclaraciones del propio conjunto.

## 3. Carpeta apuntes-sistemas-main

Procedencia declarada por el proyecto: repositorio [ivecanahi/apuntes-sistemas](https://github.com/ivecanahi/apuntes-sistemas). El [README local](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/README.md) identifica los apuntes como obra de **Ivett Anahi Zaragocin Mora** y describe las fotografías como sus apuntes manuscritos.

### PDF semanales

| Clave | Archivo en apuntes/ | Páginas | Aporte principal |
|---|---|---|---|
| I11 | [Semana1_Parte1.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana1_Parte1.pdf) | 7 | Embebidos, CPU, buses, arquitecturas, memorias y conversiones |
| I12 | [Semana1_Parte2.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana1_Parte2.pdf) | 5 | Lógica, máscaras, ejercicios, ensamblador y E/S AVR |
| I21 | [Semana2_Parte1.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana2_Parte1.pdf) | 5 | ESP32, GPIO, placa, W1TS/W1TC, alimentación y arranque |
| I22 | [Semana2_Parte2.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana2_Parte2.pdf) | 6 | Tipos, promociones, millis, variables y antirrebote |
| I31 | [Semana3_Parte1.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana3_Parte1.pdf) | 3 | Timer, prescaler, resolución y ejercicios de alarma |
| I32 | [Semana3_Parte2.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana3_Parte2.pdf) | 5 | Autorrecarga, límites, API 3.x, ISR y bandera |
| I41 | [Semana4_Parte1.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana4_Parte1.pdf) | 4 | Sondeo, detección GPIO, flancos y attachInterrupt |
| I42 | [Semana4_Parte2.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana4_Parte2.pdf) | 4 | ISR, caché, IRAM, DRAM y volatile |
| I43 | [Semana4_Parte3.pdf](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes/Semana4_Parte3.pdf) | 4 | Bandera, contador, rebote, integración y preguntas de control |

### Fotografías inspeccionadas

Todas están en `apuntes-sistemas-main/apuntes-mano/`. Las claves son de este cuaderno; no se les asigna una fecha de clase a partir del nombre del escaneo.

| Clave | Archivo | Uso efectivo de la inspección |
|---|---|---|
| I-F01 | [CamScanner 10-11-26 09.38n_1.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_1.jpg) | Conceptos iniciales de sistemas digitales y embebidos |
| I-F02 | [CamScanner 10-11-26 09.38n_2.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_2.jpg) | Comparación de dispositivos y descripción de memorias |
| I-F03 | [CamScanner 10-11-26 09.38n_3.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_3.jpg) | CPU, memoria, direcciones y esquema de integración |
| I-F04 | [CamScanner 10-11-26 09.38n_4.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_4.jpg) | Esquema de buses, ciclo CPU, unidades, ALU y flip-flop; ampliada para lectura |
| I-F05 | [CamScanner 10-11-26 09.38n_5.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_5.jpg) | Harvard/Von Neumann y clasificación de memorias |
| I-F06 | [CamScanner 10-11-26 09.38n_6.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_6.jpg) | Conversiones, máscaras y relación chip/módulo/placa; ampliada para lectura |
| I-F07 | [CamScanner 10-11-26 09.38n_7.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_7.jpg) | DDR, PORT, PIN y comparación AVR/ESP32 |
| I-F08 | [CamScanner 10-11-26 09.38n_8.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_8.jpg) | Contador, reloj APB, prescaler y resolución |
| I-F09 | [CamScanner 10-11-26 09.38n_9.jpg](../carpeta-donde-hay-archivos-para-que-te-guies/apuntes-sistemas-main/apuntes-mano/CamScanner%2010-11-26%2009.38n_9.jpg) | Cálculo de 10 000 ticks/s, alarma 2500 y conversión inconsistente; ampliada para comprobarla |

## 4. Documentación técnica adicional

Estas fuentes oficiales se consultaron para **verificar y precisar**; no se presentan como apuntes de clase. Fecha de consulta del cuaderno: **11 de noviembre de 2026**. Las páginas `latest` o `stable` pueden cambiar; para el punto conflictivo de `reload_count` se indican tags concretos del código.

| Clave | Documento oficial | Uso concreto |
|---|---|---|
| D1 | [Microchip, ATmega328P datasheet, 7810D](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf) | Memorias, CPU AVR, mapa E/S, DDR/PORT/PIN, escritura en PINB e instrucciones |
| D2 | [Espressif, ESP32 Series Datasheet](https://documentation.espressif.com/esp32_datasheet_en.html) | Alcance de variantes, CPU, SRAM, alimentación y características de GPIO |
| D3 | [ESP-IDF, GPIO & RTC GPIO, ESP32](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/gpio.html) | Pines de entrada, strapping, Flash/PSRAM y restricciones GPIO36/39 |
| D4 | [Espressif, ESP32 Technical Reference Manual](https://documentation.espressif.com/ESP32_Technical_Reference_Manual_en.pdf) | Organización GPIO/IO_MUX y semántica W1TS/W1TC |
| D5 | [Arduino-ESP32, Timer API](https://docs.espressif.com/projects/arduino-esp32/en/latest/api/timer.html) | Número de timers y API 3.x; se detectó la discrepancia del cuarto argumento |
| D6 | [Arduino-ESP32, GPIO API](https://docs.espressif.com/projects/arduino-esp32/en/latest/api/gpio.html) | pinMode, digitalRead/Write, attachInterrupt y detachInterrupt |
| D7 | [ESP-IDF, Interrupt Allocation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/intr_alloc.html) | Condiciones de una ruta de interrupción segura cuando la caché está deshabilitada |
| D8 | [ESP-IDF 5.1.4, FreeRTOS: Critical Sections](https://docs.espressif.com/projects/esp-idf/en/v5.1.4/esp32/api-reference/system/freertos_idf.html#critical-sections) | Limitación de deshabilitar interrupciones locales y uso de spinlocks compartidos |
| D9 | [ESP-IDF, Application Startup Flow](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/startup.html) | ROM, bootloader, inicialización y llegada a app_main |
| D10 | [Arduino-ESP32 3.3.0, cores/esp32/main.cpp](https://github.com/espressif/arduino-esp32/blob/3.3.0/cores/esp32/main.cpp) | Relación app_main, loopTask, setup y loop |
| D11 | [ESP-IDF 5.1.4, GPTimer](https://docs.espressif.com/projects/esp-idf/en/v5.1.4/esp32/api-reference/peripherals/gptimer.html) | Frecuencia/resolución, contexto ISR y significado de reload_count como valor de recarga |
| D12 | [Arduino-ESP32 3.0.7, esp32-hal-timer.c](https://github.com/espressif/arduino-esp32/blob/3.0.7/cores/esp32/esp32-hal-timer.c) y [el mismo archivo en 3.3.0](https://github.com/espressif/arduino-esp32/blob/3.3.0/cores/esp32/esp32-hal-timer.c) | Comprobación de timerAlarm, selección de divisor, arranque del timer y paso directo del valor de recarga a GPTimer |
| D13 | [Arduino-ESP32, Migration from 2.x to 3.0](https://docs.espressif.com/projects/arduino-esp32/en/latest/migration_guides/2.x_to_3.0.html) | Firmas de timerBegin y timerAttachInterrupt; sustitución de timerAlarmWrite/Enable |

**Discrepancia documentada:** D5 describe `reload_count` como cantidad de recargas; D12 lo asigna al campo homónimo de GPTimer, que D11 define como valor del contador al recargar. Los ejemplos se limitan a recarga en cero y explican el comportamiento según las implementaciones comprobadas. No se modificó ninguna fuente para ocultar la diferencia.

## 5. Recursos gráficos propios

| Semana | SVG creados | Propósito |
|---|---|---|
| 1 | arquitectura_microcontrolador, buses, ciclo_cpu | Mostrar bloques y flujo de instrucciones |
| 2 | led_pulsador, rebote | Explicar conexiones verificables y estabilidad |
| 3 | temporizador, autorrecarga | Relacionar reloj, conteo, alarma y repetición |
| 4 | flancos, isr_bandera_loop | Mostrar transiciones y comunicación entre contextos |

Los esquemas son funcionales o conceptuales según sus leyendas. No representan resultados experimentales ni copian el diseño gráfico de las fuentes. Se evitó un mapa físico universal de DevKit V1 porque la referencia no identifica de forma inequívoca cada variante de placa.
