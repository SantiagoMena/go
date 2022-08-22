# Tutorial: Primeros pasos con Go

Tabla de contenido

- [x] [Requisitos previos](#requisitos-previos)
- [x] [Instalar Go](#instalar-go)
- [x] [Escribiendo algo de código](#escribiendo-algo-de-código)
- [x] [Llamando código en un paquete externo](#llamando-código-en-un-paquete-externo)
- [x] [Escribiendo más código](#escribiendo-más-código)

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
- Importa el popular [paquete](https://pkg.go.dev/fmt/) `fmt`, que contiene funciones para formatear texto, incluido imprimirlo en la consola. Este terminal es uno de [la librería estándar](https://pkg.go.dev/std) de paquetes que obtienes al instalar Go.
- Implementa una función `main`para imprimir un mensaje en la consola. La función `main` se ejecuta por defecto cuando corres el paquete `main`.

6. Corre tu código para ver el saludo.

        $ go run .
        Hello, World!

El [comando](https://pkg.go.dev/cmd/go#hdr-Compile_and_run_Go_program) `go run` es uno de los muchos comandos de go que usaras para hacer cosas en Go. Usa el siguiente comando para obtener una lista de los otros comandos:

        $ go help

## Llamando código en un paquete externo

Cuando necesitas que tu código haga algo que podría haberse implementado por otra persona, puedes buscar un paquete que contenga esas funciones para usarlas en tu código.

1. Haz que el mensaje que se imprime algo más interesante con una función de un módulo externo.

    1. Visite de [pkg.go.dev](pkg.go.dev) y [busca el paquete "quote"](https://pkg.go.dev/search?q=quote)
    2. Encuentra y da click en el [paquete](https://pkg.go.dev/rsc.io/quote) `rsc.io/quote` en los resultados del buscador (si ves `rsc.io/quote/v3`, ignóralo por ahora)
    3. En la sección **Documentation**, debajo de **Index**, nota la lista de funciones que puedes llamar desde tu código. Usaras la función `Go`.
    4. En a parte superior la pagina, nota que el paquete `quote` está incluido en el módulo `rsc.io/quote`.

Puedes usar el sitio [pkg.go.dev](pkg.go.dev) para encontrar módulos publicados que contengan funciones que puedas usar entu propio código. Los paquetes son publicados cómo módulos -- como `rsc.io/quote`-- donde otro pueden usarlos. Los módulos son mejorados en el tiempo con cada versión nueva y puedes actualizar tu código usando mejorando las versiones.

2. En tu código de Go, importa el paquete `rsc.io/quote` y agrega una llamada a la función Go en tu código.

Tu código debería ser el siguiente

        package main

        import "fmt"

        import "rsc.io/quote"

        func main() {
            fmt.Println(quote.Go())
        }

3. Agrega nuevos requisitos del módulo.

Go agregara el módulo `quote` como requerimiento, así como un archivo `go.sum`para la autenticación del módulo. Para saber más, mira [Autenticando Módulos](https://go.dev/ref/mod#authenticating) en las Referencias de Módulo de Go.

        $ go mod tidy
        go: finding module for package rsc.io/quote
        go: found rsc.io/quote in rsc.io/quote v1.5.2

4. Corre tu código para ver el mensaje generado por la función que haz llamado.

        $ go run .
        Don't communicate by sharing memory, share memory by communicating.

Ten en cuenta que tu código llama la función `Go`, imprimiendo un inteligente mensaje sobre comunicación.

Cuando corriste `go mod tidy`, se encontró y descargó el módulo `rsc.io/quote` que contiene el paquete que importaste. Por defecto, se descargo la última versión.

## Escribiendo más código

Con está rápida introducción, tendrás Go instalado y aprendiste algunas cosas básicas. Para escribir más código con otro tutorial, hecha un vistazo a [Crear un módulo de Go](../tutorial/1.Tutorial-Crear_un_modulo_de_go/README.md).
