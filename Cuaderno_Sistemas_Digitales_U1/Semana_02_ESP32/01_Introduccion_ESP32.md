# Introducción al ESP32

[Índice de la semana](README.md) · [Siguiente: GPIO](02_Arquitectura_GPIO.md)

## Dispositivo de referencia

En estas notas, **ESP32 clásico** significa el ESP32 original usado en módulos WROOM y placas DevKit V1 del material. La familia incluye variantes con núcleos, memorias, pines y periféricos diferentes. Las cifras de esta página no constituyen una ficha de todos los ESP32.

El modelo de clase incorpora CPU de 32 bits, conectividad Wi-Fi y Bluetooth, GPIO configurables, temporizadores y periféricos de comunicación. Su mayor capacidad permite hacer más tareas, pero también introduce concurrencia y una configuración más compleja.

## Comparación con el AVR

| Característica | ATmega328P del Uno clásico | ESP32 clásico del módulo WROOM estudiado |
|---|---|---|
| CPU | AVR de 8 bits, un núcleo | Xtensa LX6 de 32 bits, dos núcleos en la variante de clase |
| Reloj de referencia | 16 MHz en el Uno | Hasta 240 MHz |
| SRAM física | 2 KiB | 520 KiB internos, compartidos entre distintos usos |
| Flash | 32 KiB dentro del chip | Habitualmente 4 MiB externos dentro del módulo usado como referencia |
| EEPROM dedicada | 1 KiB | Sin equivalente dedicado; persistencia mediante Flash |
| Lógica de la placa | 5 V en el Uno | 3,3 V |
| Conectividad inalámbrica | Requiere componentes externos | Wi-Fi y Bluetooth integrados |
| Selección de señales | Funciones alternativas ligadas a pines concretos | Muchas señales pasan por matriz GPIO e IO_MUX |

La SRAM física no está totalmente disponible para un arreglo de usuario: el sistema, las pilas y las bibliotecas ocupan memoria. «32 bits» tampoco significa que toda tarea sea cuatro veces más rápida que en AVR.

## Chip, módulo y placa

El **chip** es el circuito integrado ESP32. El **módulo**, por ejemplo WROOM, reúne el chip y componentes como Flash, cristal y antena. La **placa de desarrollo** agrega regulación, conexión USB-serie, botones y terminales accesibles.

| Parte de una DevKit V1 habitual | Función |
|---|---|
| Módulo WROOM | Procesamiento, memoria externa del módulo y radio |
| Conector USB | Alimentación y enlace con el computador mediante el puente serie |
| CP2102 o CH340, según placa | Puente USB a UART |
| Regulador | Obtiene 3,3 V a partir de la alimentación admitida |
| EN | Habilitación/reset del chip |
| BOOT | Relacionado con GPIO0 y el modo de descarga |
| Hileras de terminales | Acceso a alimentación y parte de los GPIO |

El nombre comercial «DevKit V1» no garantiza un único esquema. En las fuentes aparece una versión de 30 pines; existen otras distribuciones. Por eso este cuaderno presenta **grupos funcionales** y no inventa un pinout físico universal.

## Alimentación

En la placa de referencia, el USB aporta 5 V y el regulador produce 3,3 V. Los GPIO utilizan lógica de **3,3 V** y no admiten señales de 5 V directamente. Si se comunica con un dispositivo de otra tensión, se necesita una adaptación apropiada y referencia de GND común.

Un terminal rotulado VIN o 5V debe comprobarse en el esquema de la placa antes de usarlo. Tampoco se debe asumir que se pueden conectar simultáneamente varias fuentes sin revisar su circuito de alimentación.

**Base de clase:** R, pp. 6 y 11–13; K05; I11, p. 6; I21, pp. 1–4; fotografías I-F06 e I-F07. **Verificación adicional:** D1–D3. Véase [Fuentes](../FUENTES.md).
