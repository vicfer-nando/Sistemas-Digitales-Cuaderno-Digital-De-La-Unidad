# Fundamentos de interrupciones

[Índice de la semana](README.md) · [Siguiente: GPIO y flancos](02_Interrupciones_GPIO.md)

## Un aviso que cambia temporalmente el flujo

Una **interrupción** permite que una fuente de hardware solicite atención a la CPU. Cuando se acepta, se guarda el contexto necesario, se ejecuta la rutina de servicio y después se reanuda el contexto interrumpido.

En la semana 3 la fuente era la alarma de un timer. En esta semana la fuente es un evento del **periférico GPIO**, como un flanco en una entrada. Se habla de interrupción externa porque el evento viene de una señal exterior al microcontrolador.

La descripción «el hardware llama a una función» es una forma abreviada de recordar el mecanismo: hay detección, lógica de interrupción y código del controlador antes de llegar al callback del usuario.

## Sondeo frente a interrupción

El **sondeo**, o polling, consiste en leer repetidamente un estado desde el programa principal. Si se lee a los instantes 0 ms y 100 ms, un pulso que empiece a los 30 ms y termine a los 40 ms puede pasar completamente desapercibido.

| Aspecto | Sondeo | Interrupción |
|---|---|---|
| Detección | El código consulta el pin | El periférico detecta el evento configurado |
| Trabajo continuo | Requiere comprobaciones periódicas | Requiere configurar y atender avisos |
| Pulso entre consultas | Puede perderse | Puede detectarse si cumple los requisitos del hardware |
| Complejidad | Menor para entradas lentas | Exige ISR y coordinación de datos compartidos |
| Trabajo posterior | Se realiza desde el flujo principal | Conviene diferirlo al flujo principal |

Un botón lento suele resolverse bien con sondeo frecuente y antirrebote. La interrupción interesa cuando el evento debe detectarse independientemente de la frecuencia de lectura del superlazo. Tampoco sustituye un periférico específico de conteo o captura para señales muy rápidas.

## Secuencia de atención

```text
Cambio del pin → detección GPIO → solicitud de interrupción
              → atención de la ISR → retorno al contexto interrumpido
```

La ISR interrumpe la ejecución en **un núcleo**. En un ESP32 de dos núcleos no congela necesariamente todo el sistema. Puede interrumpir otra tarea que esté ejecutándose, no exclusivamente `loop()`.

## Latencia y duración

La **latencia** es el tiempo entre el evento y el comienzo de su atención. La **duración de la ISR** es el tiempo que tarda el manejador. Mantener la ISR corta reduce interferencias con otros trabajos y permite atender nuevos eventos con menos demora.

Que el programa esté en un `delay()` de la tarea principal no implica que las interrupciones estén deshabilitadas. El evento puede registrar información y el procesamiento de `loop()` ocurrirá después. Pero si llegan varios eventos y solo existe una bandera, se agrupan.

**Precisión respecto a las fuentes:** no se garantiza «ningún evento se pierde». Influyen el ancho de pulso, el enmascaramiento de interrupciones, la capacidad del periférico, el tiempo de atención y la forma de guardar los avisos. Para observar el efecto, deben distinguirse flancos físicos, ISR ejecutadas y acciones finalmente realizadas.

## Comprobación

**¿Por qué una ISR no debería esperar a que se suelte el botón?** Mantendría ocupado el contexto de interrupción durante un tiempo humano, impidiendo una atención breve. Se registra el evento y la liberación se evalúa después.

**Base de clase:** R, pp. 18–19; K08; I41, pp. 1–2; I42, pp. 1–2. **Precisión técnica:** D7–D8. Véase [Fuentes](../FUENTES.md).
