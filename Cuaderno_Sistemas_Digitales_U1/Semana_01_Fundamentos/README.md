# Semana 1 · Fundamentos de microcontroladores

[Índice general](../README.md)

La meta es comprender cómo un programa termina controlando un pin. El recorrido empieza en los bloques del microcontrolador y termina en los registros de E/S del AVR.

## Apuntes de la semana

1. [Microcontroladores y sistemas embebidos](01_Microcontroladores.md).
2. [CPU, buses, ciclo de instrucciones y arquitecturas](02_Arquitectura_CPU.md).
3. [Flash, SRAM, EEPROM y capacidad](03_Memorias.md).
4. [Sistemas binario, decimal y hexadecimal](04_Sistemas_Numericos.md).
5. [Operaciones lógicas, máscaras y desplazamientos](05_Operaciones_Bits.md).
6. [Instrucciones básicas del ensamblador AVR](06_Ensamblador_AVR.md).
7. [DDRB, PORTB, PINB y E/S digitales](07_Registros_AVR.md).
8. [Apoyo: electrónica básica y almacenamiento de un bit](08_Electronica_Basica.md).

## Autoevaluación

**¿1024 direcciones significan siempre 1024 bytes?** Solo si cada dirección identifica un byte; hay que conocer el tamaño de cada posición.

**¿16 MHz garantizan 16 millones de instrucciones por segundo?** No. Depende de los ciclos por instrucción y del flujo ejecutado.

**¿Cómo apagar PB4 sin alterar los otros bits?** Con `PORTB &= ~(1 << PB4);`, considerando también la concurrencia si otra rutina modifica el puerto.

## Repaso de la semana

- Un microcontrolador reúne CPU, memoria y periféricos para controlar una aplicación.
- La ALU opera, los registros guardan datos inmediatos y el PC orienta la búsqueda de instrucciones.
- Direcciones identifica el destino; datos transporta el contenido; control define la operación.
- Harvard separa programa y datos; Von Neumann los comparte en su modelo básico.
- Flash conserva el programa, SRAM sostiene el trabajo y EEPROM conserva pequeños datos.
- Un byte tiene ocho bits; cada dígito hexadecimal representa cuatro.
- OR establece bits, AND con máscara invertida los borra y XOR los alterna.
- En AVR, DDR configura, PORT escribe o activa pull-up y PIN lee el nivel del puerto.
