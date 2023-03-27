# Tutorial: Introducción a los espacios de trabajo multimódulo

Tabla de contenido

- [x] [Requisitos previos](#requisitos-previos)
- [x] [Cree un módulo para su código](#cree-un-módulo-para-su-código)
- [x] [Crear el espacio de trabajo](#crear-espacio-de-trabajo)
- [x] [Descargar y modificar el módulo golang.org/x/example](#descargar-y-modificar-el-módulo-golang.org/x/example)
- [x] [Más información sobre los espacios de trabajo](#más-información-sobreespacios-de-trabajo)

Este tutorial presenta los fundamentos de los espacios de trabajo multimódulo en Go. Con los espacios de trabajo multimódulo, puedes decirle al comando Go que estás escribiendo código en múltiples módulos al mismo tiempo y fácilmente construir y ejecutar código en esos módulos.

En este tutorial, crearás dos módulos en un espacio de trabajo multimódulo compartido, harás cambios en esos módulos, y verás los resultados de esos cambios en una compilación.

## Requisitos previos

- Una instalación de Go 1.18 o posterior.
- Una herramienta para editar tu código. Cualquier editor de texto que tengas funcionará bien.
- Un terminal de comandos. Go funciona bien usando cualquier terminal en Linux y Mac, y en PowerShell o cmd en Windows.
- Este tutorial requiere go1.18 o posterior. Asegúrate de haber instalado Go en Go 1.18 o posterior usando los enlaces en [go.dev/dl]('go.dev/dl').

## Cree un módulo para su código

Para empezar, crea un módulo para el código que vas a escribir.

1. Abra un símbolo del sistema y cambie a su directorio principal.

  En Linux o Mac:

    $ cd

  En Windows:

    C:\> cd %HOMEPATH%

El resto del tutorial mostrará un $ como prompt. Los comandos que utilices funcionarán también en Windows.

2. Desde la línea de comandos, cree un directorio para su código llamado espacio de trabajo.

    $ mkdir workspace
    $ cd workspace

3. Inicializa el módulo

Nuestro ejemplo creará un nuevo módulo hello que dependerá del módulo golang.org/x/example.

Crea el módulo hello:

    $ mkdir hello
    $ cd hello
    $ go mod init example.com/hello
    go: creating new go.mod: module example.com/hello

Añade una dependencia del módulo golang.org/x/example usando go get.

    $ go get golang.org/x/example

Crea hello.go en el directorio hello con el siguiente contenido:

    package main

    import (
        "fmt"

        "golang.org/x/example/stringutil"
    )

    func main() {
        fmt.Println(stringutil.Reverse("Hello"))
    }

Ahora, ejecuta el programa hello:

    $ go run example.com/hello
    olleH

## Crear el espacio de trabajo

En este paso, crearemos un archivo go.work para especificar un espacio de trabajo con el módulo.

### Inicializar el espacio de trabajo

En el directorio del espacio de trabajo, ejecute

    $ go work init ./hello

El comando `go work init` le dice a go que cree un archivo `go.work` para un espacio de trabajo que contenga los módulos en el directorio `./hello`.

El comando go produce un archivo go.work con el siguiente aspecto:

    go 1.18

    use ./hello

El archivo go.work tiene una sintaxis similar a go.mod.

La directiva go indica a Go con qué versión de Go debe interpretarse el archivo. Es similar a la directiva go en el archivo `go.mod`.

La directiva `use` le dice a Go que el módulo en el directorio `hello` debe ser el módulo principal cuando se hace una compilación.

Así que en cualquier subdirectorio del espacio de trabajo el módulo estará activo.

### Ejecute el programa en el directorio del espacio de trabajo

En el directorio del área de trabajo, ejecute

    $ go run example.com/hello
    olleH

El comando Go incluye todos los módulos en el área de trabajo como módulos principales. Esto nos permite referirnos a un paquete en el módulo, incluso fuera del módulo. Ejecutar el comando `go run` fuera del módulo o del espacio de trabajo resultaría en un error porque el comando go no sabría qué módulos usar.

A continuación, vamos a añadir una copia local del módulo `golang.org/x/example` al espacio de trabajo. A continuación, añadiremos una nueva función al paquete `stringutil` que podremos utilizar en lugar de `Reverse`.

## Descargar y modificar el módulo golang.org/x/example

En este paso, descargaremos una copia del repositorio Git que contiene el módulo golang.org/x/example, lo añadiremos al espacio de trabajo, y luego le añadiremos una nueva función que utilizaremos desde el programa hello.

1. Clonar el repositorio

  Desde el directorio del espacio de trabajo, ejecuta el comando git para clonar el repositorio:

    $ git clone https://go.googlesource.com/example
    Cloning into 'example'...
    remote: Total 165 (delta 27), reused 165 (delta 27)
    Receiving objects: 100% (165/165), 434.18 KiB | 1022.00 KiB/s, done.
    Resolving deltas: 100% (27/27), done.

2. Añade el módulo al espacio de trabajo

    $ go work use ./example

El comando `go work use` añade un nuevo módulo al archivo `go.work`. Ahora tendrá el siguiente aspecto:

    go 1.18

    use (
        ./hello
        ./example
    )

El módulo ahora incluye tanto el módulo `example.com/hello` como el módulo golang.org/x/example.

Esto nos permitirá utilizar el nuevo código que escribiremos en nuestra copia del módulo `stringutil` en lugar de la versión del módulo en la caché de módulos que descargamos con el comando `go get`.

3. Añade la nueva función.

Añadiremos una nueva función para poner en mayúsculas una cadena al paquete `golang.org/x/example/stringutil`.

Crea un nuevo archivo llamado `toupper.go` en el directorio `workspace/example/stringutil` con el siguiente contenido:

    package stringutil

    import "unicode"

    // ToUpper uppercases all the runes in its argument string.
    func ToUpper(s string) string {
        r := []rune(s)
        for i := range r {
            r[i] = unicode.ToUpper(r[i])
        }
        return string(r)
    }

4. Modifica el programa hola para utilizar la función.

Modifique el contenido de `workspace/hello/hello.go` para que contenga lo siguiente:

    package main

    import (
        "fmt"

        "golang.org/x/example/stringutil"
    )

    func main() {
        fmt.Println(stringutil.ToUpper("Hello"))
    }

### Ejecutar el código en el espacio de trabajo

En el directorio del espacio de trabajo, ejecute

    $ go run example.com/hello
    HELLO

El comando Go encuentra el módulo example.com/hello especificado en la línea de comandos en el directorio `hello` especificado por el archivo `go.work`, y de forma similar resuelve la importación `golang.org/x/example` usando el archivo `go.work`.

`go.work` se puede utilizar en lugar de añadir directivas replace para trabajar a través de múltiples módulos.

Dado que los dos módulos están en el mismo espacio de trabajo es fácil hacer un cambio en un módulo y utilizarlo en otro.

### Paso futuro

Ahora, para liberar correctamente estos módulos tendríamos que hacer una liberación del módulo `golang.org/x/example`, por ejemplo a v0.1.0. Esto se hace normalmente etiquetando un commit en el repositorio de control de versiones del módulo. Consulte la documentación del flujo de trabajo de liberación del módulo para más detalles. Una vez que la liberación está hecha, podemos aumentar el requisito en el módulo `golang.org/x/example` en hello/go.mod:

    cd hello
    go get golang.org/x/example@v0.1.0

De este modo, el comando go puede resolver correctamente los módulos fuera del espacio de trabajo.

## Más información sobre los espacios de trabajo

El comando go tiene un par de subcomandos para trabajar con espacios de trabajo además de go work init que vimos anteriormente en el tutorial:

go work use [-r] [dir] añade una directiva use al archivo go.work para dir, si existe, y elimina el directorio use si el directorio argumento no existe. La opción -r examina los subdirectorios de dir de forma recursiva.
go work edit edita el archivo go.work de forma similar a go mod edit
go work sync sincroniza las dependencias de la lista de construcción del espacio de trabajo en cada uno de los módulos del espacio de trabajo.
Vea Espacios de Trabajo en la Referencia de Módulos Go para más detalles sobre espacios de trabajo y ficheros go.work.