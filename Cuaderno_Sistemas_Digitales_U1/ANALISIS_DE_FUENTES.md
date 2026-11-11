# Análisis y distribución de las fuentes

[Índice general](README.md) · [Catálogo de fuentes](FUENTES.md)

## Inventario y método

Se recorrió toda la carpeta `carpeta-donde-hay-archivos-para-que-te-guies` y sus subcarpetas. Se encontraron **28 archivos: 18 PDF, 9 JPG y 1 Markdown**. No se encontraron fuentes de código, presentaciones ni otros materiales en esa carpeta al revisarla.

Los PDF suman **85 páginas físicas**: 22 del resumen, 20 del conjunto de ocho clases y 43 de los nueve PDF semanales. Se extrajo texto de todos y se revisaron visualmente las páginas mediante imágenes de contacto; también se examinaron las nueve fotografías y se ampliaron las que sustentan cálculos y diagramas relevantes.

En K01–K05, una parte importante de las explicaciones y los manuscritos está incorporada como imagen y no aparece en la extracción de texto. Por eso la lectura visual fue necesaria. Las fotografías son de contraste: no se copiaron al cuaderno. Cuando una escritura tenue no permitía afirmar un detalle, se priorizaron el texto legible y los ejemplos coincidentes, sin reconstruir frases dudosas.

## Qué aporta cada conjunto

| Conjunto | Aporte efectivo | Uso en el cuaderno |
|---|---|---|
| R, resumen de Richard Ariel Chamba Santin | Recorre ocho clases; añade aclaraciones y un formulario con ejercicios | Comprobar cobertura y correcciones; conservar datos de ejemplos para recalcularlos |
| K01–K08, conjunto de Katheryn Contento | Organiza cada clase y muestra manuscritos; K06–K08 desarrollan métodos y cálculos en texto | Secuencia conceptual y contraste de AVR, pulsadores, timers e interrupciones |
| I11–I43 y fotografías, conjunto de Ivett Anahi Zaragocin Mora | División en cuatro semanas; esquemas, API 3.x y ejemplos de programación | Organización semanal, tipos de datos, concurrencia y antirrebote |

La coincidencia entre apuntes de la misma asignatura no equivale a tres experimentos independientes ni valida automáticamente una cifra. Se distinguieron datos de placa, modelos didácticos, resultados calculados y aclaraciones técnicas.

## Distribución adoptada

| Semana | Fuentes centrales | Organización resultante |
|---|---|---|
| 1 | R, pp. 2–10; K01–K04; I11–I12; I-F01 a I-F07 | Ocho temas: fundamentos, CPU, memoria, bases, bits, ensamblador, E/S AVR y apoyo electrónico |
| 2 | R, pp. 11–14; K05–K06; I21–I22; I-F06–I-F07 | Seis temas: plataforma, GPIO, arranque, tipos, conexiones y pulsadores |
| 3 | R, pp. 15–17 y 20–21; K07; I31–I32; I-F08–I-F09 | Cinco temas: timer, reloj/divisor, alarma, API/ISR y ejercicios |
| 4 | R, pp. 18–19; K08; I41–I43 | Cinco temas: interrupción, flancos, ISR/memoria, comunicación y antirrebote |

Los temas repetidos se introducen una vez y se enlazan desde las aplicaciones. La semana 3 utiliza el patrón de aviso y remite a la 4 para su explicación detallada. Los dos antirrebotes se conservan porque uno usa sondeo y el otro notificación por interrupción.

## Discrepancias y decisiones explícitas

| Punto de contraste | Dónde se observa | Tratamiento |
|---|---|---|
| EPROM frente a EEPROM | R, p. 5, aclara el rótulo del manuscrito; primeras notas de memorias | Se explica la diferencia y se utiliza EEPROM para la memoria persistente del AVR |
| Byte y borrar un bit | R, p. 10, señala errores del manuscrito | Se fija 8 bits por byte y CBI para borrar PB4 |
| KB/MB con base 1024 | R, p. 5; I11, p. 5; I-F04 | Se conserva el valor del ejemplo y se rotula KiB/MiB para distinguir prefijos decimales |
| Tick de 10 kHz | I-F09 muestra sustituciones inconsistentes; R, p. 16 y K07, p. 2 lo aclaran | Se desarrolla $0{,}0001\,s=0{,}1\,ms$ y 2500 ticks = 0,25 s |
| Microprocesador = solo CPU; embebido = una sola cosa | I11, p. 1 y K01 | Se mantiene la comparación introductoria con alcance explícito, sin convertirla en definición universal |
| Transistor y significado del bit | I12, p. 1 y notas iniciales | Se distingue el bit abstracto de su representación física; el nivel de salida depende del circuito |
| Una línea C = una instrucción | I12, p. 3 | Se expresa como una posible traducción optimizada de un caso, no regla general |
| pinMode configura toda la matriz | I21, p. 1 | Se aclara que un periférico requiere su propia configuración, contrastada con D3–D4 y D6 |
| Pines, Flash de 4 MB y LED integrado | I21, pp. 2–4 | Se limita al módulo/placa del ejemplo; no se dibuja un pinout físico universal |
| delay detiene la CPU / timer avisa a la hora exacta | I31, p. 1 | Se diferencia bloqueo de una tarea, evento de hardware y latencia de atención |
| Divisor máximo 65 535 | I32, p. 2 | Se registra que el core 3.0.7 comprobado contempla hasta 65 536; ejercicios sin conflicto |
| Toda ISR sin IRAM_ATTR falla | I42, p. 3 | Se explica la condición de caché y las dependencias; el atributo aislado no certifica seguridad, según D7 |
| volatile como protección suficiente | Esquemas simples de I32 e I43; R y K08 ya advierten límites | Se añade sección crítica para copiar/limpiar y se aclara la agrupación de eventos |
| noInterrupts como protección general | K08, p. 2 | Los ejemplos usan un portMUX compartido conforme a D8 para el ESP32 con dos núcleos |
| Diez pulsaciones producen más de diez avisos sin filtro | I43, p. 4 | Se diferencia flancos, ISR y mensajes; una bandera y un loop lento pueden agrupar eventos |
| reload_count en timerAlarm | D5 lo describe como cantidad de recargas; implementación D12 y GPTimer D11 usan valor de recarga | Se documenta el desacuerdo y se basa el ejemplo en recarga cero, sin prometer un número de repeticiones |

## Material adicional y límites

La documentación oficial se utilizó para verificar registros, pines, versiones de API, arranque, seguridad de ISR y secciones críticas. Esas precisiones se marcan en el tema correspondiente y se catalogan por separado en [Fuentes](FUENTES.md).

R menciona presentaciones como S01, S2, S3 y S4 y archivos manuscritos con otros nombres. **Esos archivos originales no están entre las referencias locales**. Se cita el PDF realmente disponible, no las diapositivas que ese PDF cita. No se atribuyen fechas de clase propias a partir de fechas presentes en apuntes ajenos.

Los ejemplos se presentan como material didáctico, sin afirmar ejecución en placa ni mediciones. La revisión de sintaxis y sus límites se registran en [Verificación](VERIFICACION.md).
