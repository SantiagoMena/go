# Tutorial: Crear un módulo de Go

Tabla de contenido
[Requisitos previos](#requisitos-previos)
[Crea un módulo que otros puedan usar](#crea-un-módulo-que-otros-puedan-usar)

Esta es la primer parte de un tutorial que introduce algunas características fundamentales del lenguaje Go. Si estás iniciando con Go, asegúrate de dar un vistazo al [Tutorial: Primeros pasos con Go](../1.Tutorial-Primeros_Pasos/README.md), el cual hace una introducción al comando go, módulos de Go, y código simple en Go.

En este tutorial crearas dos módulos. El primero es una librería que está destinada a ser importada por otras librerías o aplicaciones. El segundo es la aplicación de llamadas que usara el primero.

Esta secuencia de tutoriales incluye siete temas breves cada uno ilustra diferentes partes del lenguaje

1. Crear un módulo -- Escribe un pequeño módulo con funciones que puedan ser llamadas desde otro módulo
2. [Llama tu código desde otro módulo](../2.2.Tutorial-Llama_tu_codigo_desde_otro_modulo/README.md) -- Importa y usa tu nuevo módulo.
3. [Retorna y gestiona errores](../2.3.Tutorial-Retorna_y_gestiona_un_error/)
4. [Retorna un saludo aleatorio](../2.4.Tutorial-Retorna_un_saludo_aleatorio/)
5. [Retorna saludos para multiples personas](../2.5.Tutorial-Retorna_saludos_para_multiples_personas/)
6. [Agrega un test](../2.6.Tutorial-Agrega_un_test/)
7. [Compila e instala la aplicación](../2.7.Tutorial-Compila_e_instala_la_aplicacion/)

## Requisitos previos

- **Algo de experiencia programando.** El código acá es bastante sencillo, pero ayudara conocer algo acerca de funciones.
- **Una herramienta para editar código.** Cualquier editor de texto con el que haz trabajado. La mayoría de editores de texto tienen buen soporte para Go. El más popular es VSCode (gratis), GoLand(pago) y Vim(gratis).
- **Una terminal de comandos.** Go  funciona bien usando cualquier terminal en Linux y Mac, y e PowerShell o cmd en Windows.

## Crea un módulo que otros puedan usar
