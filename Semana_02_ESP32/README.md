# Semana 2 · Introducción y manejo del ESP32

[Índice general](../README.md) · [Semana anterior](../Semana_01_Fundamentos/README.md)

La meta es llevar la relación programa–registro–pin al ESP32 clásico y leer un pulsador de manera confiable. Los ejemplos utilizan GPIO16 para un LED externo y GPIO21 para el pulsador, como en el material; se presupone una placa con módulo WROOM que deje disponibles esos pines.

## Apuntes de la semana

1. [ESP32, AVR, chip, módulo y placa](01_Introduccion_ESP32.md).
2. [Arquitectura, periféricos, GPIO, W1TS y W1TC](02_Arquitectura_GPIO.md).
3. [Arranque, programación y superlazo](03_Arranque_Programacion.md).
4. [Tipos de datos, signo, promociones y tiempo](04_Tipos_Datos.md).
5. [Entradas, salidas, LED y resistencias](05_Entradas_Salidas.md).
6. [Pulsadores, rebote y antirrebote con millis()](06_Pulsadores_Antirrebote.md).

## Autoevaluación

**¿Por qué no conectar 5 V directamente a un GPIO?** La lógica del ESP32 clásico es de 3,3 V y sus entradas no son tolerantes a 5 V.

**¿Por qué puede fallar INPUT_PULLUP en GPIO34?** Ese grupo de pines es solo de entrada y carece de pull-up/pull-down interna.

**¿Un botón mantenido en LOW representa muchas pulsaciones?** No; se detecta una transición del estado ya filtrado, no se incrementa en cada vuelta del programa.

## Repaso de la semana

- Chip, módulo y placa describen niveles distintos de integración.
- El ESP32 clásico y el ATmega328P comparten la idea de E/S mediante registros, pero tienen arquitecturas distintas.
- La matriz GPIO permite enrutar muchas señales; cada pin conserva sus restricciones.
- Los 5 V del USB se regulan para el sistema de 3,3 V; no son el nivel lógico de los GPIO.
- W1TS y W1TC modifican solo los bits donde se escribe 1.
- setup() inicializa una vez por arranque y loop() se repite dentro del entorno Arduino.
- El tamaño de int cambia entre AVR y ESP32; los tipos de ancho fijo hacen explícita la intención.
- Un pulsador necesita un nivel definido y antirrebote; millis() permite validar sin detener el superlazo.
