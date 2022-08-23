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

Inicia creando un módulo. En un módulo, puedes reunir uno o más paquetes relacionados para un conjunto discreto y util de funciones. Por ejemplo, puedes crear un módulo con paquetes que tengan funciones o para hacer análisis financiero para que otros escriban aplicaciones financieras y usen tu trabajo. Para más acerca de desarrollo de módulos, mira [Desarrollando y publicando módulos](https://go.dev/doc/modules/developing)

El código de Go es agrupado en paquetes, y los paquetes son agrupados en módulos. Tu módulo especifica las dependencias necesarias para correr tu código, incluida la versión de Go y otr conjunto de módulos requeridos.

A medida que agregas o mejoras funcionalidades en tu módulo, publicas nuevas versiones del módulo. Los desarrolladores escriben código que llama  funciones en tu módulo y pueden importar las actualizaciones de los paquetes del módulo y probarlos con la nueva versión antes de pasarlos su uso en producción.

1. Abre una terminal de comandos y dirigete a tu directorio `home`

En Linux o Mac:

        cd

En Windows:

        cd %HOMEPATH%

2. Crea un directorio `greetings` para el código fuente de tu módulo en Go.

3. Inicia tu módulo usando el [comando](https://go.dev/ref/mod#go-mod-init) `go mod init`

Corre el comando `go mod init`, proporcionando la ruta de tu módulo -- en esta ocasión, usa `example.com/greetings`. Si publicas el módulo, es *necesario* que la ruta sea desde el lugar desde donde va a ser descargado el módulo por las herramientas de Go. Puede ser el repositorio de tu código.

Para saber más sobre como nombrar tu moulo con el directorio de del módulo, mira [Manegando depedencias](https://go.dev/doc/modules/managing-dependencies#naming_module)

        $ go mod init example.com/greetings
        go: creating new go.mod: module example.com/greetings

El comando `go mod init` crea un archivo `go.mod` para rastrear las dependencias de tu código. Hasta ahora, el archivo incluye solo el nombre de tu módulo y la versión de Go que tu código soporta. Pero mientras añades dependencias, el archivo `go.mod` enumerara las versiones de las cuales tu codigo depende. Esto mantiene el desarrollo reproducible y te da el control directo sobre las versiones del módulo a usar.

4. En tu editor de texto, crea un archivo para escribir tu código nombralo `greetings.go`.

5. Pega el siguiente código dentro de tu archivo `greetings.go` y guardalo.

        package greetings

        import "fmt"

        // Hello returns a greeting for the named person.
        func Hello(name string) string {
            // Return a greeting that embeds the name in a message.
            message := fmt.Sprintf("Hi, %v. Welcome!", name)
            return message
        }

Este es el primer código de tu módulo. El retorna un saludo para cualquier que realice una llamada. Escribirás las llamadas a la función en el siguiente paso.

En este código, tu haz:

- Declarado el paquete `greetings` para reunir funciones relacionadas.
- Implementado una función `Hello`que retorna un saludo
    Esta función toma un parametro `name` de tipo `string`. LA función además retorna un `string`. En Go, una función que su nombre inicia con una mayuscula puede ser llamada por una función que no se encuentre en el mismo paquete. Esto se conoce en go como un `exported name`. Para sabes más acerca de los los, mira [Nombres Exportados](https://go.dev/tour/basics/3) en el tur de Go.

    <img src="function-syntax.png" alt="sintxis de función">

- Declarado una variable `message` para guardar el saludo

    En Go, el operador `:=` es un atajo para declarar e inicializar una vairable en una linea (Go usa la variable a la derecha para determinar el tipo de variable). Tomando la ruta larga, deberías haber escrito algo así:

        var message string
        message = fmt.Sprintf("Hi, %v. Welcome!", name)    

- Usado la [función](https://pkg.go.dev/fmt/#Sprintf) `Sprintf` del paquete `fmt` para crear un mensaje. El primer argumento es de formato string, y `Sprintf` substituye el valor del parámetro `name` por el formato verb `%v`. Insertado el valor del parámetro `nombre` completamente en el texto del saludo.
- Retornado el saludo de texto a llamador(caller).

En el siguiente paso, llamaras a la esta función desde otro módulo.

[LLama al código desde otro módulo](../2.2.Tutorial-Llama_tu_codigo_desde_otro_modulo/README.md)