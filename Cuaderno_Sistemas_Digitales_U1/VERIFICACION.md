# Verificación del cuaderno

[Índice general](README.md) · [Fuentes](FUENTES.md)

Revisión realizada el **11 de noviembre de 2026**. El alcance de esta etapa es contenido Markdown y diagramas SVG.

## Estructura y cobertura

| Carpeta | Temas desarrollados | Recursos | Cobertura revisada |
|---|---|---|---|
| Semana_01_Fundamentos | 8 archivos temáticos y README | 3 SVG | MCU/MPU, embebidos, CPU, ALU, PC, buses, ciclo, reloj, Harvard/Von Neumann, memorias, unidades, bases, lógica, máscaras, desplazamientos, AVR y DDR/PORT/PIN; apoyo electrónico adicional |
| Semana_02_ESP32 | 6 archivos temáticos y README | 2 SVG | Características y comparación AVR, chip/módulo/DevKit, alimentación, GPIO, periféricos, matriz, W1TS/W1TC, arranque, setup/loop, tipos, E/S, LED, resistencias, pulsadores y antirrebote |
| Semana_03_Temporizadores | 5 archivos temáticos y README | 2 SVG | Contador, timer/delay, frecuencia, prescaler, resolución, ticks, alarma, API 2.x/3.x, autorrecarga, desbordamiento, ISR/callback, variables compartidas, formulario y ejercicios |
| Semana_04_Interrupciones | 5 archivos temáticos y README | 2 SVG | Sondeo, GPIO, RISING/FALLING/CHANGE, attachInterrupt, ISR, IRAM_ATTR, volatile, comunicación con loop, banderas, contadores, antirrebote y errores frecuentes |

El resultado contiene **32 archivos Markdown y 9 SVG**: 24 temas, cuatro índices semanales, índice general, fuentes, análisis comparativo y este registro. Cada índice semanal termina con entre ocho y diez ideas de repaso, precedidas por preguntas de autoevaluación con respuesta.

## Revisión de archivos y navegación

Se comprobaron los títulos principales, las separaciones de encabezados, el cierre de bloques de código y la existencia de contenido en cada archivo. Los temas tienen desarrollo real, ejemplos o comparaciones y procedencia indicada. No hay secciones sin desarrollar ni marcadores de trabajo pendiente.

Los destinos de enlaces e imágenes locales se resolvieron desde el archivo que los contiene, decodificando las rutas con espacios o caracteres acentuados. Se verificaron tanto los enlaces internos del cuaderno como los que abren las referencias locales. Los enlaces externos corresponden a las fuentes consultadas; no se copian como archivos de salida.

Se revisaron las repeticiones para conservar solo las necesarias: el patrón de aviso se aplica a timer y GPIO, mientras la explicación de concurrencia se concentra en la semana 4. Los ejercicios se reúnen en un archivo específico. Las variantes de antirrebote identifican sus diferencias y límites.

## Diagramas

Los nueve SVG se analizaron como XML válido y se renderizaron a PNG temporales para inspección visual. Se revisaron conexiones, etiquetas, márgenes y legibilidad. Se utilizaron puntas de flecha geométricas para que el resultado no dependa de la interpretación de marcadores SVG del conversor.

Se comprobó que los textos quedan dentro de sus lienzos. Los recursos tienen fondo blanco, texto negro y líneas negras o grises. Son diagramas originales y están enlazados mediante rutas relativas desde los temas. Las imágenes temporales de revisión quedaron fuera del cuaderno y fuera de la carpeta de referencias.

## Fórmulas y resultados

Se efectuaron **39 comprobaciones aritméticas programáticas** con enteros o aritmética decimal: períodos a 16 y 80 MHz; cuatro pares frecuencia/divisor; duraciones de tick; siete casos de alarma; tamaños de arreglos y memoria; módulo sin signo; corriente del LED y pull-up; desgaste estimado; límites de ocho y dieciséis bits; conversiones y operaciones de máscaras/W1TS/W1TC.

Además se recalcularon el período de 60 Hz, el retorno de millis() y la escala del contador de 64 bits. Resultados de control:

| Caso | Resultado comprobado |
|---|---|
| 16 MHz | 62,5 ns por ciclo |
| 80 MHz / 80 | 1 MHz; 1 µs por tick |
| 10 kHz y 2500 ticks | 100 µs por tick; 250 ms |
| 20 kHz y 5000 ticks | 50 µs por tick; 250 ms; divisor 4000 con entrada de 80 MHz |
| 1 MHz y 500 000 ticks | 0,5 s; 2 avisos/s con recarga cero |
| 16 bits a 1 MHz, desde cero | Desbordamiento tras 65,536 ms |
| LED rojo idealizado, 3,3 V, Vf 2 V y 330 Ω | Aproximadamente 3,94 mA |
| 64 bits a 1 MHz | Aproximadamente 584 542 años de 365,25 días para el retorno completo |

Las fórmulas explicitan su modelo: conteo ascendente, inicio/recarga en cero y ticks enteros en los casos básicos. Se distingue el primer intervalo cuando hay otro inicio, el período con otra recarga, la frecuencia de ticks y la de avisos. No se aplicó una regla universal de «restar uno» a la alarma.

## Código: comprobaciones y límites

Se revisaron **18 bloques C/C++ con un analizador sintáctico Tree-sitter**, normalizando solo los atributos de plataforma para que el analizador reconozca su posición. No se detectaron errores sintácticos en esos bloques. Las instrucciones del fragmento AVR se revisaron por mnemónico, operandos y correspondencia con el mapa del ATmega328P.

También se contrastaron las firmas de la API con documentación oficial, se identificaron los fragmentos y los seis sketches independientes, y se revisaron máscaras, pines, inicialización y transferencia de datos compartidos. Los ejemplos de interrupción usan secciones críticas breves y dejan el trabajo lento fuera de la ISR.

**Límite de validación:** no se encontró arduino-cli ni compilador AVR/ESP32 en el entorno. La revisión sintáctica no sustituye una compilación con un core concreto, el enlazado ni una prueba en placa. No se declara ejecución experimental. La configuración IRAM depende también del core y sus bibliotecas, como se explica en el tema correspondiente.

## Conservación de originales y formatos

Se calcularon huellas **SHA-256 de los 28 archivos de referencia antes de la elaboración** y se compararon nuevamente al finalizar. Coincidieron todas las rutas y huellas: no hubo archivos de referencia modificados, añadidos ni eliminados.

En `Cuaderno_Sistemas_Digitales_U1` se comprobaron únicamente extensiones **.md y .svg**. No se generaron documentos Word, PDF ni proyectos de programación como entregables. La extracción y las imágenes de inspección se trabajaron en una carpeta temporal externa al proyecto.
