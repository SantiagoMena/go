# Tutorial: Introducción a los espacios de trabajo multimódulo

Tabla de contenido

- [x] [Requisitos previos](#requisitos-previos)
- [x] [Cree un módulo para su código](#cree-un-módulo-para-su-código)
- [ ] [Crear el espacio de trabajo](#crear-espacio-de-trabajo)
- [ ] [Descargar y modificar el módulo golang.org/x/example](#descargar-y-modificar-el-módulo-golang.org/x/example)
- [ ] [Más información sobre los espacios de trabajo](#más-información-sobreespacios-de-trabajo)

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

El comando `go work init` le dice a go que cree un archivo `go.work` para un espacio de trabajo que contenga los módulos en el directorio ./hello.

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

