# ISR, IRAM_ATTR y volatile

[Índice de la semana](README.md) · [Siguiente: banderas y eventos](04_Banderas_Eventos.md)

## Una ISR debe terminar pronto

La **ISR** es código ejecutado en contexto de interrupción. Allí conviene guardar información mínima: una bandera, un incremento protegido o un dato pequeño. El trabajo extenso se deja para `loop()` u otra tarea.

Se evitan `delay()`, `Serial.print()`, asignaciones dinámicas, esperas y comunicaciones de alto nivel dentro de los ejemplos de ISR. Pueden bloquear, consumir demasiado tiempo o depender de servicios que no son apropiados para ese contexto. El **watchdog** detecta ciertos bloqueos; una ISR larga puede contribuir a reinicios, pero el mensaje concreto debe analizarse antes de atribuir una causa única.

| Dentro de la ISR del cuaderno | Fuera de la ISR |
|---|---|
| Marcar un aviso | Imprimir su descripción |
| Actualizar un contador bajo protección | Hacer cálculos o mostrar resultados |
| Guardar el mínimo dato necesario | Acceder a redes, pantallas o almacenamiento |

## Flash, IRAM y DRAM

La **Flash** conserva el programa al apagar. En el ESP32 clásico se accede a gran parte de su código mediante caché. La **SRAM interna** contiene regiones empleadas para instrucciones y para datos.

**IRAM** significa RAM de instrucciones: aloja código que la CPU ejecuta desde memoria interna. **DRAM** significa RAM de datos: aloja variables. No son «otro tipo de EEPROM» ni vuelven persistentes los datos.

```cpp
void IRAM_ATTR alEvento() {
  // Trabajo breve; las dependencias también deben ser apropiadas para ISR.
}
```

`IRAM_ATTR` pide situar la función en una sección de IRAM. No es una orden de prioridad ni un mecanismo de exclusión mutua.

## Lo que IRAM_ATTR no garantiza

**Precisión técnica, D7:** durante operaciones de escritura/borrado de Flash puede deshabilitarse la caché. Una ruta segura en esas condiciones debe mantener en memoria interna **todo el código y los datos que utilice, incluso indirectamente**. Marcar solo la función exterior no basta si llama código en Flash o consulta constantes allí almacenadas.

Las fuentes I42 recomiendan como regla didáctica «toda ISR lleva IRAM_ATTR». En este cuaderno se mantiene el atributo en los ejemplos, pero se aclara que **su ausencia no provoca inevitablemente un fallo**: el registro y la configuración del controlador determinan si una interrupción se aplaza durante operaciones de Flash o puede atenderse entonces. El core también ofrece `ARDUINO_ISR_ATTR`, cuyo comportamiento depende de su configuración.

Llamadas como `millis()` o `digitalWrite()` dentro de una ISR no deben declararse universalmente seguras para cualquier core y configuración. Aquí se dejan fuera de los manejadores propios para conservar una ruta mínima. La ISR del ejemplo solo opera sobre datos globales de RAM y usa las primitivas de sección crítica del entorno.

## Qué expresa volatile

Una variable compartida con una ISR puede cambiar sin que el flujo normal lo muestre. `volatile` impide que el compilador trate sus accesos como si no tuvieran ese carácter observable.

```cpp
volatile bool pendiente = false;
volatile uint32_t pulsos = 0;
```

Esto **no** hace que `pulsos++` o «leer y poner en cero» sean una única operación indivisible. Tampoco constituye por sí solo sincronización general entre tareas o núcleos. Para estos ejemplos se protegen los accesos con una sección crítica compartida; otras aplicaciones pueden necesitar atómicos, colas o notificaciones.

| Uso de la variable | Decisión en los ejemplos |
|---|---|
| ISR escribe; loop lee/limpia | `volatile` y protección del protocolo de acceso |
| ISR incrementa; loop copia/limpia | `volatile` y sección crítica en ambos lados |
| Solo loop usa el dato | Variable ordinaria |
| Solo un contexto ISR usa el dato | No requiere `volatile` únicamente por estar en una ISR |

La última fila presupone que no hay otra rutina o núcleo accediendo al mismo objeto. Importa quién comparte el dato, no solo dónde fue declarado.

**Base de clase:** R, pp. 17–19; K08; I32, p. 4; I42, pp. 1–4. **Complemento técnico:** D7–D8. Véase [Fuentes](../FUENTES.md).
