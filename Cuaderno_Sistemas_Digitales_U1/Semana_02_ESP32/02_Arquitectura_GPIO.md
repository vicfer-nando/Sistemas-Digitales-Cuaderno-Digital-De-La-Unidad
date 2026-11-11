# Arquitectura, periféricos y GPIO

[Índice de la semana](README.md) · [Siguiente: arranque](03_Arranque_Programacion.md)

## GPIO y periféricos

**GPIO** significa entrada/salida de propósito general. Un pin configurado como entrada permite leer una señal; como salida permite imponer un nivel, dentro de sus límites eléctricos. Un **periférico** es un bloque de hardware dedicado a una función.

| Periférico | Para qué sirve |
|---|---|
| Temporizador | Contar ticks y generar eventos temporales |
| UART | Comunicación serie mediante transmisión y recepción |
| I²C | Bus compartido de dos señales, con dispositivos direccionados |
| SPI | Comunicación síncrona mediante reloj, datos y selección |
| ADC | Convertir tensión analógica en número |
| DAC, en el ESP32 original | Generar una tensión analógica a partir de un número |
| PWM/LEDC | Generar pulsos con ciclo de trabajo controlado |

PWM no es por sí mismo una tensión analógica continua: alterna niveles digitales. El efecto depende del receptor o del filtrado.

## IO_MUX y matriz GPIO

**IO_MUX** selecciona funciones y propiedades del terminal. La **matriz GPIO** permite enrutar muchas señales digitales de periféricos a pines compatibles. Esto ofrece flexibilidad, pero no hace que todos los periféricos funcionen en cualquier pin: las funciones analógicas y ciertas rutas rápidas tienen restricciones.

**Precisión técnica, D3–D4:** `pinMode()` prepara la función GPIO y su modo. Para una UART, SPI u otro periférico se configura también la API de ese periférico. La frase de I21 «pinMode hace los tres pasos» no debe interpretarse como que una llamada asigna automáticamente cualquier UART o SPI.

## Grupos de pines del ESP32 clásico

| Grupo | Uso o precaución |
|---|---|
| GPIO16–19, 21–23, 25–27, 32–33 | Opciones habituales para ejemplos digitales, si el módulo y el circuito los dejan libres |
| GPIO34–39 | Solo entrada; sin pull-up/pull-down interna; no todos están expuestos en cada placa |
| GPIO6–11 | Normalmente conectados a la Flash del módulo WROOM; reservarlos |
| GPIO0, 2, 5, 12, 15 | Pines de strapping: niveles muestreados durante el arranque |
| GPIO1 y GPIO3 | UART0 habitual de carga y mensajes; pueden entrar en conflicto con ese uso |

GPIO16 y GPIO17 pueden estar comprometidos por memoria en otros módulos, como ciertas variantes WROVER. El ESP32 clásico no tiene GPIO20, 24 ni 28–31: la numeración no es una lista continua. **Precisión adicional, D3:** GPIO36 y GPIO39 tienen restricciones de interrupción al combinar determinados usos de ADC o radio con modos de sueño; se prefiere GPIO21 en los ejemplos.

## Registros equivalentes a AVR

| Función | AVR | ESP32 clásico, banco GPIO0–31 |
|---|---|---|
| Habilitar salida | DDRB | `GPIO_ENABLE_REG` y variantes de set/clear |
| Escribir salida | PORTB | `GPIO_OUT_REG`, W1TS y W1TC |
| Leer entrada | PINB | `GPIO_IN_REG` |
| Configurar pull-up | PORTB en modo entrada | Campos de configuración de IO_MUX |

GPIO32–39 pertenecen al segundo banco. Un bit válido en un registro no convierte en salida a un pin que físicamente es solo entrada.

## W1TS y W1TC

**Write 1 to set** establece los bits indicados. **Write 1 to clear** los borra. En ambos, escribir 0 deja el bit sin cambio.

| Registro de salida antes | Escritura | Operación | Salida después |
|---|---|---|---|
| `0101` | `0010` | W1TS | `0111` |
| `0111` | `0010` | W1TC | `0101` |

Una operación convencional `salida |= mascara` puede leer una copia, modificarla y escribirla. Si otra rutina cambia un bit entre esas etapas, la escritura puede perder ese cambio. W1TS/W1TC evitan esa lectura previa para los bits seleccionados. **No coordinan por sí solos una secuencia de varias operaciones ni resuelven dos órdenes incompatibles sobre el mismo bit**.

**Fragmento para Arduino-ESP32 3.x y ESP32 clásico**. Primero se configura GPIO16 con `pinMode(16, OUTPUT)` en `setup()`; después se puede llamar a estas funciones:

```cpp
#include <Arduino.h>
#include "soc/soc.h"
#include "soc/gpio_reg.h"
void encenderGPIO16() {
  REG_WRITE(GPIO_OUT_W1TS_REG, 1UL << 16);
}
void apagarGPIO16() {
  REG_WRITE(GPIO_OUT_W1TC_REG, 1UL << 16);
}
```

Cada llamada escribe una máscara directamente en el registro de acción. Para comenzar, `digitalWrite()` expresa el propósito con menos dependencia del mapa de registros.

**Base de clase:** R, pp. 11–12; K05; I21, pp. 1–4. **Verificación adicional:** D2–D4 y D6. Véase [Fuentes](../FUENTES.md).
