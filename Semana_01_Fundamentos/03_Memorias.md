# Memorias y capacidad

[Índice de la semana](README.md) · [Siguiente: sistemas numéricos](04_Sistemas_Numericos.md)

## Tres memorias con propósitos distintos

**Volátil** significa que el contenido se pierde al retirar la alimentación. **No volátil** significa que se conserva sin energía, dentro de las condiciones de retención del dispositivo.

| Memoria | ¿Conserva datos al apagar? | Uso habitual en el AVR | Precaución |
|---|---|---|---|
| Flash | Sí | Programa y constantes | Escritura/borrado por mecanismos específicos y con desgaste |
| SRAM | No | Variables, pila y datos de trabajo | Es limitada; excederla produce fallos |
| EEPROM | Sí | Configuración o pequeños datos persistentes | Evitar reescrituras innecesarias |

En un termostato, el programa se guarda en Flash, la lectura actual en SRAM y la consigna persistente puede guardarse en EEPROM. La CPU **ejecuta**; la memoria **almacena**.

**Aclaración de las fuentes:** en R, p. 5, se advierte que una descripción manuscrita rotulada EPROM corresponde a EEPROM. Una EPROM clásica se borra con luz ultravioleta; una EEPROM se borra eléctricamente. Aquí se utiliza el nombre EEPROM para los datos persistentes del ATmega328P.

## Capacidades del dispositivo de estudio

El ATmega328P dispone de **32 KiB de Flash, 2 KiB de SRAM y 1 KiB de EEPROM**. Parte de la Flash puede reservarse para el cargador de arranque. Las capacidades instaladas no equivalen siempre al espacio disponible para variables o programa de usuario.

En el ESP32 clásico, la Flash suele estar fuera del chip, dentro del módulo. Los 4 MiB citados en clase corresponden a una configuración frecuente del WROOM; hay variantes. El ESP32 no ofrece una EEPROM dedicada equivalente a la del AVR; puede conservar configuración en Flash mediante servicios como NVS.

## Bits, bytes y prefijos

| Unidad | Equivalencia |
|---|---|
| Bit | Un valor binario: 0 o 1 |
| Byte, B | 8 bits |
| KiB | $1024\,\mathrm{B}$ |
| MiB | $1024\,\mathrm{KiB}=1\,048\,576\,\mathrm{B}$ |
| kB, prefijo decimal | $1000\,\mathrm{B}$ |
| MB, prefijo decimal | $1\,000\,000\,\mathrm{B}$ |

Las fuentes usan «KB = 1024 bytes», una convención común en clase. En este cuaderno se escribe **KiB** cuando la cantidad es binaria. No confundir **Mb** (megabits) con **MB** (megabytes).

## Ejercicio: ¿cabe el arreglo?

En el AVR del Uno, un `int` ocupa dos bytes. Para `int datos[2000]`:

$$
\text{tamaño}=\text{elementos}\times\text{bytes por elemento}
=2000\times2=4000\,\mathrm{B}.
$$

La SRAM total es $2\times1024=2048\,\mathrm{B}$. **No cabe**, incluso antes de reservar espacio para la pila y las demás variables. En ESP32, el mismo arreglo de `int` suele requerir $2000\times4=8000\,\mathrm{B}$; cambiar de plataforma también cambia el tamaño de ciertos tipos.

## Escritura y desgaste

La ficha del ATmega328P da como referencias de resistencia **10 000 ciclos de Flash y 100 000 de EEPROM** bajo sus especificaciones. No son cifras universales para todas las memorias.

Si una misma celda se reescribiera 300 veces por segundo, $100\,000/300\approx333{,}3\,\mathrm{s}$, unos $5{,}56\,\mathrm{min}$. Es una estimación didáctica de ciclos consumidos, **no una medición ni una predicción exacta de avería**. Conviene guardar solo cuando cambia el dato y con una frecuencia razonable.

**Base de clase:** R, pp. 5–6; K03; I11, pp. 4–6; fotografías I-F02, I-F05 e I-F06. **Verificación adicional:** D1 y D2. Véase [Fuentes](../FUENTES.md).
