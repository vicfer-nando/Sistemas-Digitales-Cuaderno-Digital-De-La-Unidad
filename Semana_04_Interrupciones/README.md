# Semana 4 · Interrupciones externas

[Índice general](../README.md) · [Semana anterior](../Semana_03_Temporizadores/README.md)

La meta es reaccionar a cambios de una entrada sin consultar continuamente el pin para descubrir cada evento. El hardware detecta el cambio, una ISR deja información mínima y el programa principal decide qué hacer.

## Apuntes de la semana

1. [Interrupciones y comparación con sondeo](01_Fundamentos_Interrupciones.md).
2. [GPIO, flancos y attachInterrupt()](02_Interrupciones_GPIO.md).
3. [ISR, IRAM_ATTR y volatile](03_ISR_IRAM_Volatile.md).
4. [Comunicación ISR–loop: banderas y contadores](04_Banderas_Eventos.md).
5. [Antirrebote mediante interrupciones y errores frecuentes](05_Antirrebote_Interrupciones.md).

## Autoevaluación

**¿Qué flanco representa presionar un botón con pull-up?** FALLING, porque pasa de alto a bajo.

**¿Cuatro asignaciones de true equivalen a cuatro avisos conservados?** No. Una bandera conserva una condición pendiente; para cantidad se necesita un contador u otro mecanismo.

**¿volatile evita perder una actualización entre leer y limpiar?** No. Se protege la operación compuesta o se utiliza una primitiva apropiada.

## Repaso de la semana

- El sondeo consulta; una interrupción permite que el periférico solicite atención.
- La respuesta de una ISR tiene latencia y no implica que todo pulso posible se conserve.
- Con pull-up, FALLING corresponde a presionar y RISING a soltar.
- CHANGE detecta ambos flancos y también los producidos por rebote.
- Se configura primero el pin y después se registra la ISR en setup().
- La ISR debe ser breve; impresiones, esperas y trabajo pesado se trasladan a loop().
- IRAM_ATTR ubica código en IRAM; no garantiza por sí solo que toda la ruta sea segura sin caché.
- volatile informa de accesos externos, pero la coordinación necesita protección adicional.
- Una bandera agrupa eventos; un contador conserva cantidad dentro de sus límites.
- El antirrebote temporal y la validación de estabilidad cumplen funciones distintas.
