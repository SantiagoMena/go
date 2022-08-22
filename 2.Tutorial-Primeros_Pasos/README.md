# Tutorial: Primeros pasos con Go

Tabla de contenido

- [ ] [Requisitos previos](#requisitos-previos)
- [ ] [Instalar Go](#instalar-go)
- [ ] [Escribiendo algo de código](#escribiendo-algo-de-código)
- [ ] [Llamando código en un paquete externo](#llamando-código-en-un-paquete-externo)
- [ ] [Escribiendo más código](#escribiendo-más-código)

En este tutorial, tendrás una breve introducción a la programación en Go. En el camino tendrás que:

- Instalar Go (si aun no lo haz hecho).
- Escribir algo de simple código "Hola, mundo".
- Usar el comando `go` para correr el código.
- Usar la herramienta de paquetes de Go para encontrar paquetes que puedas usar en tu propio código.
- Llamar funciones de módulos externos

   **Nota:** Para otros tutoriales, mira [Tutoriales](../DOC.md#primeros-pasos)

## Requisitos previos

- **Algo de experiencia programando.** El código acá es bastante sencillo, pero ayudara conocer algo acerca de funciones.
- **Una herramienta para editar código.** Cualquier editor de texto con el que haz trabajado. La mayoría de editores de texto tienen buen soporte para Go. El más popular es VSCode (gratis), GoLand(pago) y Vim(gratis).
- **Una terminal de comandos.** Go  funciona bien usando cualquier terminal en Linux y Mac, y e PowerShell o cmd en Windows.

## Instalar Go

Sigue los pasos de [descargar e instalar](../1.Instalando_Go/README.md)

## Escribiendo algo de código

Empieza con "Hello, World".

1. Abre una terminal de comando y dirígete a tu directorio `home`

    En Linux o Mac

        cd

    En Windows

        cd %HOMEPATH%

2. Crea un directorio `hello` para tu primer código fuente en Go.

Por ejemplo, usa los siguientes comandos:

        mkdir hello
        cd hello

3. Habilita el seguimiento de dependencias para tu código.

Cuando tu código importa paquetes contenidos en otros módulos, tu gestionas esas dependencias en el código de tu propio módulo. Ese módulo es definido por un archivo `go.mod` que rastrea los módulos que proporcionan esos paquetes. Ese archivo `go.mod` permanece con tu código, incluso en el código fuente de tu repositorio.

Para habilitar el rastreo de dependencias para tu código creando el archivo `go.mod`, corre el [comando](https://go.dev/ref/mod#go-mod-init) `go mod init`, proporcionando el nombre del módulo donde tu código estará. El nombre del directorio es el directorio de del módulo.

En el desarrollo actual, la ruta del módulo suele ser el repositorio local donde tu código fuente estará. Por ejemplo, la ruta del módulo puede ser `github.com/mimodulo`. Si tu plan es publicar el módulo para que lo usen otros, la ruta del módulo *debe ser* una dirección desde donde las herramientas de Go puedan descargar tu módulo. Para saber más sobre los nombres de los módulo con una ruta de módulo, mira [Manejando dependencias](https://go.dev/doc/modules/managing-dependencies#naming_module).

Para el proposito de este tutorial, sólo usa `example/hello`

        $ go mod init example/hello
        go: creating new go.mod: module example/hello

4. En tu editor de texto, crea un archivo `hello.go` en el que escribir tu código.

5. Pega el siguiente código en tu archivo `hello.go` y guardalo.

        package main

        import "fmt"

        func main() {
            fmt.Println("Hello, World!")
        }

Este es el código de Go. En este código:

- Declara un paquete `main` (un paquete es la forma de agrupar funciones, y está compuesto por todos los archivos del directorio ).
- Importa el popular [paquete](https://pkg.go.dev/fmt/) `fmt`, que contiene funciones para formatear texto, incluido imprimirlo en la consola. Este terminal es uno de [la libreria estandard](https://pkg.go.dev/std) de paquetes que obtienes al instalar Go.
- Implementa una función `main`para imprimir un mensaje en la consola. La función `main` se ejecuta por defecto cuando corres el paquete `main`.

6. Corre tu código para ver el saludo.

        $ go run .
        Hello, World!

El [comando](https://pkg.go.dev/cmd/go#hdr-Compile_and_run_Go_program) `go run` es uno de los muchos comandos de go que usaras para hacer cosas en Go. Usa el siguiente comando para obtener una lista de los otros comandos:

        $ go help

## Llamando código en un paquete externo

## Escribiendo más código