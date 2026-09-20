# Videopic

Circuito basado en el **microcontrolador PIC16F877A**, programado en **lenguaje ensamblador (ASM)**, capaz de generar una señal de vídeo **PAL en blanco y negro** para mostrar caracteres y gráficos en una pantalla de televisión.

El proyecto combina **electrónica digital, programación de microcontroladores y generación de señales de vídeo**, utilizando directamente los recursos del PIC16F877A para generar los sincronismos y la información de imagen.

---

## 🎯 Descripción

Videopic es un sistema autónomo de generación de vídeo basado en un **PIC16F877A funcionando a 20 MHz**.

El firmware genera mediante software los **sincronismos horizontales y verticales** necesarios para producir una señal de televisión PAL y utiliza el periférico **SSP** del microcontrolador para transmitir los datos de vídeo.

La información mostrada se almacena en la memoria del PIC y se representa mediante un conjunto de caracteres definidos en el propio código ensamblador.

---

## 🖥️ Modos de vídeo

El sistema dispone de dos modos de visualización seleccionables mediante la etiqueta `MODO_PANT`.

```asm
MODO_PANT EQU .0    ; Modo de vídeo 0
MODO_PANT EQU .1    ; Modo de vídeo 1
```

### Modo 0

Pantalla organizada en:

```text
20 × 8 caracteres
```

La memoria de pantalla se divide en dos zonas, correspondientes a la parte superior e inferior de la imagen.

El modo dispone de **31 caracteres gráficos independientes**, definidos mediante patrones de **8 × 8 píxeles**.

### Modo 1

Pantalla organizada en:

```text
7 × 11 caracteres
```

Al igual que el modo 0, utiliza caracteres gráficos de **8 × 8 píxeles**.

Este modo incorpora, además del juego de caracteres alfanumérico, varios gráficos personalizados.

---

## 🔤 Juego de caracteres

El programa incluye un conjunto de caracteres definidos directamente mediante tablas de datos `RETLW`.

El juego de caracteres incluye:

* Espacio.
* Letras de la `A` a la `Z`.
* Número `1`.
* Número `8`.
* Patrón de rejilla.
* Gráficos personalizados.

Cada carácter está formado por una matriz de:

```text
8 × 8 píxeles
```

Los píxeles se representan mediante bits individuales:

```asm
RETLW B'00111100'
RETLW B'01000010'
RETLW B'01000010'
RETLW B'01111110'
RETLW B'01000010'
RETLW B'01000010'
RETLW B'00000000'
```

Este sistema permite definir nuevos caracteres y gráficos modificando directamente sus patrones binarios.

---

## 🎮 Gráficos personalizados

El juego de caracteres del modo 1 incluye tres patrones destinados a formar un gráfico de **Jet Set Willy**.

```text
JET SET WILLY (1)
JET SET WILLY (2)
JET SET WILLY (3)
```

Los tres caracteres se combinan para formar una figura de mayor tamaño utilizando varias posiciones de la pantalla.

El proyecto también incorpora referencias gráficas procedentes del **Sinclair ZX81**, utilizadas como inspiración para parte del juego de caracteres.

---

## 📝 Personalización del texto

Los textos que aparecen en pantalla están definidos mediante las etiquetas:

```asm
TEXTO
TEXTO2
```

El programa convierte posteriormente los caracteres almacenados en estas tablas al formato utilizado internamente por el generador de vídeo.

Por ejemplo:

```asm
DT "  jet  "
DT "  set  "
DT " willy "
```

También es posible insertar caracteres gráficos definidos en las tablas del programa.

---

## 📺 Generación de vídeo

La señal de vídeo se genera directamente mediante software utilizando el PIC16F877A.

El programa implementa:

* Sincronismo horizontal.
* Sincronismo vertical.
* Pulsos igualadores.
* Líneas de vídeo.
* Líneas en negro.
* Generación de los datos de imagen.
* Temporizaciones mediante instrucciones del microcontrolador.

El sincronismo vertical se genera mediante las rutinas:

```text
PIGUALADORES
PVERTICALESALM
SYNCVERT
```

Mientras que la generación de las líneas de vídeo se realiza mediante rutinas como:

```text
LINEA64US
MODE0
MODE1
```

Las temporizaciones se implementan mediante instrucciones `NOP` y llamadas a rutinas de retardo, teniendo en cuenta la frecuencia de reloj del microcontrolador.

---

## ⚙️ Microcontrolador

El proyecto utiliza un:

**Microchip PIC16F877A**

Configuración principal:

| Parámetro           | Valor             |
| ------------------- | ----------------- |
| Microcontrolador    | PIC16F877A        |
| Frecuencia de reloj | **20 MHz**        |
| Lenguaje            | **ASM**           |
| Sistema de vídeo    | **PAL**           |
| Imagen              | Blanco y negro    |
| Tamaño de carácter  | **8 × 8 píxeles** |
| Modos de vídeo      | **2**             |

---

## 🧠 Organización de la memoria

El programa utiliza parte de la memoria RAM del PIC como **memoria de pantalla**.

### Modo 0

La pantalla se divide en dos zonas:

```text
Banco 0
┌──────────────────────┐
│      Parte superior  │
│       20 × 4         │
└──────────────────────┘

Banco 1
┌──────────────────────┐
│      Parte inferior  │
│       20 × 4         │
└──────────────────────┘
```

Cada posición de la pantalla contiene el código correspondiente al carácter que debe mostrarse.

### Modo 1

Utiliza parte de la memoria del banco 0 para almacenar la información de la pantalla:

```text
7 × 11 caracteres
```

---

## 🔄 Flujo de generación de imagen

El funcionamiento general puede resumirse de la siguiente manera:

```text
        ┌──────────────────────┐
        │      PIC16F877A      │
        │        20 MHz        │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │   Memoria de pantalla│
        │                      │
        │  Caracteres / datos  │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │ Tablas de caracteres │
        │       8 × 8          │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │ Generación de vídeo  │
        │                      │
        │ H + V + imagen       │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │     Señal PAL B/N    │
        └──────────┬───────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │       TELEVISOR      │
        └──────────────────────┘
```

---

## ⚡ Alimentación

El circuito está diseñado para funcionar con una tensión de alimentación aproximada de:

```text
9 V ───────── 18 V
```

Puede utilizarse una fuente o adaptador multitensión adecuado dentro de este rango, teniendo en cuenta el consumo del circuito.

---

## 💻 Software

El firmware está desarrollado íntegramente en **lenguaje ensamblador para la familia PIC**.

Entre las principales rutinas del programa se encuentran:

```text
START
START_MODE0
START_MODE1
MODE0
MODE1
SYNCVERT
LINEA64US
BYTEPANT
BYTEPANT1
CONV_CHAR
CONV_CHAR2
BORRAPANT
```

Estas rutinas se encargan de la inicialización del microcontrolador, generación de vídeo, gestión de la memoria de pantalla, conversión de caracteres y generación de los sincronismos.

---

## 📚 Conceptos aplicados

El proyecto reúne diferentes conceptos de electrónica y sistemas embebidos:

* Programación de microcontroladores.
* Lenguaje ensamblador.
* Arquitectura PIC.
* Manipulación directa de registros.
* Gestión de memoria RAM.
* Tablas de datos mediante `RETLW`.
* Generación de señales de vídeo mediante software.
* Generación de sincronismos horizontales y verticales.
* Temporización mediante ciclos de instrucción.
* Comunicación mediante el periférico SSP.
* Representación gráfica mediante mapas de bits.
* Electrónica digital.
* Diseño de circuitos electrónicos.

---

## 📌 Estado del proyecto

Proyecto desarrollado sobre **PIC16F877A a 20 MHz**, capaz de generar vídeo **PAL monocromo** y mostrar caracteres y gráficos personalizados en una pantalla de televisión.

El código fuente permite modificar el **modo de vídeo**, el contenido mostrado y los patrones gráficos directamente desde el programa ensamblador.
