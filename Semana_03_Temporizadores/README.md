# Semana 3 · Temporizadores de hardware

[Índice general](../README.md) · [Semana anterior](../Semana_02_ESP32/README.md)

La meta es pasar de «quiero esperar cierto tiempo» a una configuración de reloj, conteo y alarma. Los cálculos usan el modelo de clase: reloj de entrada de 80 MHz, conteo ascendente y recarga en cero, salvo indicación expresa.

## Apuntes de la semana

1. [Temporizador, contador y comparación con delay()](01_Fundamentos_Temporizadores.md).
2. [Frecuencia, prescaler, resolución y ticks](02_Frecuencia_Prescaler.md).
3. [Alarmas, autorrecarga y desbordamiento](03_Alarmas_Autorrecarga.md).
4. [Arduino-ESP32 3.x, callbacks e interrupciones](04_Interrupciones_Timer.md).
5. [Formulario y ejercicios resueltos](05_Formulas_Ejercicios.md).

## Autoevaluación

**A 1 MHz, ¿qué alarma produce 250 ms?** $A=0{,}25\times1\,000\,000=250\,000$ ticks.

**¿10 kHz significan 10 000 interrupciones por segundo?** Solo si se produce una interrupción por tick. Con alarma de 2500 ticks y recarga en cero hay 4 avisos por segundo.

**¿La alarma requiere que el contador se desborde?** No. Compara con un valor programado, normalmente muy inferior al máximo.

## Repaso de la semana

- El contador es un registro de hardware; no una variable incrementada por loop().
- El prescaler divide la frecuencia de entrada y cambia la duración del tick.
- $f_{tick}=f_{entrada}/N$ y $\Delta t=1/f_{tick}$.
- Con conteo y recarga desde cero, $t=A/f_{tick}$ y $A=t f_{tick}$.
- La frecuencia de avisos es $f_{tick}/A$, distinta de la frecuencia del contador.
- Autorrecarga y desbordamiento tienen causas diferentes.
- El ESP32 original tiene cuatro temporizadores generales de 64 bits; otras variantes difieren.
- Arduino-ESP32 3.x configura la frecuencia con timerBegin(frecuencia); 2.x usa otra firma.
- La ISR registra el aviso; loop() hace el trabajo y los accesos compartidos se protegen.
