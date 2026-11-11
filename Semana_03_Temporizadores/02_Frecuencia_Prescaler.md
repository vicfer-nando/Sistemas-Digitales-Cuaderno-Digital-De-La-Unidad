# Frecuencia, prescaler y resolución

[Índice de la semana](README.md) · [Siguiente: alarmas](03_Alarmas_Autorrecarga.md)

## Tres cantidades que no deben mezclarse

La **frecuencia de entrada** llega desde una fuente de reloj. El **prescaler** es un divisor sin unidades. La **frecuencia del contador** indica cuántos ticks ocurren por segundo después de dividir.

$$
f_{tick}=\frac{f_{entrada}}{N},\qquad
N=\frac{f_{entrada}}{f_{tick}}.
$$

En los cálculos de clase se utiliza el reloj APB a **80 MHz**. APB significa *Advanced Peripheral Bus*. Su período es $1/80\,000\,000=12{,}5\,\mathrm{ns}$. Es el reloj de entrada del ejemplo, no la frecuencia de la CPU a 240 MHz.

## Duración de un tick

La **resolución temporal** del contador es la duración de un tick:

$$
\Delta t=\frac{1}{f_{tick}}=\frac{N}{f_{entrada}}.
$$

| $f_{entrada}$ | $N$ | $f_{tick}$ | $\Delta t$ |
|---|---|---|---|
| 80 MHz | 80 | 1 MHz | 1 µs |
| 80 MHz | 2000 | 40 kHz | 25 µs |
| 80 MHz | 4000 | 20 kHz | 50 µs |
| 80 MHz | 8000 | 10 kHz | 100 µs = 0,1 ms |

**Más divisor** produce ticks más lentos: el contador tarda más en recorrer el mismo rango, pero distingue intervalos más grandes. Es mejor decir «menor resolución temporal» que afirmar que el reloj se vuelve necesariamente menos exacto: exactitud y tamaño del tick no son lo mismo.

## Cálculo guiado

**Datos:** $f_{entrada}=80\,000\,000\,\mathrm{Hz}$ y se desea un tick de $50\,\mathrm{\mu s}$.

**Fórmula:** $f_{tick}=1/\Delta t$ y $N=f_{entrada}/f_{tick}$.

**Sustitución:** $50\,\mathrm{\mu s}=0{,}00005\,\mathrm{s}$; $f_{tick}=1/0{,}00005=20\,000\,\mathrm{Hz}$; $N=80\,000\,000/20\,000=4000$.

**Resultado:** divisor 4000 y resolución 50 µs en ese modelo. El divisor debe ser realizable por el hardware y la API.

## Ticks y unidades

$$
1\,\mathrm{s}=1000\,\mathrm{ms}=1\,000\,000\,\mathrm{\mu s},\quad
1\,\mathrm{MHz}=1\,000\,000\,\mathrm{Hz}.
$$

Un tick no significa siempre un microsegundo. Solo coincide si $f_{tick}=1\,\mathrm{MHz}$. A 10 kHz, 2500 ticks duran $2500\times100\,\mathrm{\mu s}=250\,\mathrm{ms}$.

**Corrección del material:** la fotografía I-F09 contiene una conversión inconsistente entre $0{,}0001\,\mathrm{s}$ y milisegundos, y una sustitución escrita como $0{,}001\,\mathrm{s}$. El desarrollo correcto es $0{,}0001\,\mathrm{s}=0{,}1\,\mathrm{ms}$; así se obtiene 0,25 s. K07 e I31 ya presentan ese resultado corregido.

## Hardware y API 3.x

En Arduino-ESP32 3.x se solicita la frecuencia con `timerBegin(frecuencia)`. El core elige fuente y divisor; no se pasa `N` como segundo argumento. Los cálculos APB sirven para comprender el modelo, pero no deben imponerse como única fuente en cualquier configuración.

**Precisión técnica adicional, D11–D12:** I32 resume el divisor como 2 a 65 535. El core 3.0.7 comprobado acepta divisores hasta 65 536, considerando la representación especial del hardware. Para los ejercicios, 80, 2000, 4000 y 8000 están dentro de ambos límites. Una frecuencia solicitada debe contrastarse con la configuración realmente obtenida; no todo valor es realizable exactamente.

**Base de clase:** R, pp. 16 y 20–21; K07; I31, pp. 1–3; I32, p. 2; I-F09. **Verificación adicional:** D5, D11–D12. Véase [Fuentes](../FUENTES.md).
