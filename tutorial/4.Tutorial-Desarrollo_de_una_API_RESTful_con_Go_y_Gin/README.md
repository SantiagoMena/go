# Tutorial: Desarrollo de una API RESTful con Go y Gin

Tabla de contenido

- [x] [Requisitos previos](#requisitos-previos)
- [x] [Diseñar los endpoints de la API](#diseñar-los-endpoints-de-la-api)
- [x] [Cree una carpeta para su código](#cree-una-carpeta-para-su-código)
- [x] [Crear los datos](#crear-los-datos)
- [ ] [Escribir un controlador para devolver todos los elementos](#escribir-un-controlador-para-devolver-todos-los-elementos)
- [ ] [Escribir un controlador para añadir un nuevo elemento](#escribir-un-controlador-para-añadir-un-nuevo-elemento)
- [ ] [Escribir un controlador para devolver un elemento específico](#escribir-un-controlador-para-devolver-un-elemento-específico)
- [ ] [Conclusión](#conclusión)
- [ ] [Código completado](#código-completado)

Este tutorial presenta los fundamentos de la escritura de una API de servicio web RESTful con Go y el [Gin Web Framework](https://gin-gonic.com/docs/) (Gin).

Sacarás el máximo provecho de este tutorial si tienes una familiaridad básica con Go y sus herramientas. Si esta es su primera exposición a Go, por favor vea [Tutorial: Empezar con Go para una rápida introducción](../1.Tutorial-Primeros_Pasos/).

Gin simplifica muchas tareas de codificación asociadas con la construcción de aplicaciones web, incluyendo servicios web. En este tutorial, usarás Gin para enrutar peticiones, recuperar detalles de peticiones, y marshal JSON para respuestas.

En este tutorial, construirás un servidor API RESTful con dos endpoints. Tu proyecto de ejemplo será un repositorio de datos sobre discos de jazz vintage.

El tutorial incluye las siguientes secciones:

1. Diseño de los endpoints de la API.
2. Crear una carpeta para el código.
3. Crear los datos.
4. Escribir un handler para devolver todos los elementos.
5. Escribir un handler para añadir un nuevo elemento.
6. Escribe un handler para devolver un elemento específico.

## Requisitos previos

- **Una instalación de Go 1.16 o posterior.** Para obtener instrucciones de instalación, consulte [Instalación de Go](https://go.dev/doc/install).
- **Una herramienta para editar tu código.** Cualquier editor de texto que tengas funcionará bien.
- **Un terminal de comandos.** Go funciona bien usando cualquier terminal en Linux y Mac, y en PowerShell o cmd en Windows.
- **La herramienta curl.** En Linux y Mac, ya debería estar instalada. En Windows, se incluye en Windows 10 Insider build 17063 y posteriores. Para versiones anteriores de Windows, es posible que tengas que instalarla. Para más información, consulte [Tar y Curl llegan a Windows](https://learn.microsoft.com/en-us/virtualization/community/team-blog/2017/20171219-tar-and-curl-come-to-windows).

## Diseñar los endpoints de la API

Vas a construir una API que proporcione acceso a una tienda de venta de grabaciones antiguas en vinilo. Así que tendrá que proporcionar endpoints a través de los cuales un cliente pueda obtener y añadir álbumes para los usuarios.

Cuando se desarrolla una API, normalmente se empieza por diseñar los endpoints. Los usuarios de su API tendrán más éxito si los endpoints son fáciles de entender.

Estos son los endpoints que creará en este tutorial.

/albums

- GET - Obtiene una lista de todos los álbumes, devuelta como JSON.
- POST - Añade un nuevo álbum a partir de los datos de la solicitud enviados como JSON.

/albums/:id

- GET - Obtener un álbum por su ID, devolviendo los datos del álbum como JSON.
A continuación, crearás una carpeta para tu código.

## Cree una carpeta para su código

Para empezar, crea un proyecto para el código que vas a escribir.

1. Abra un símbolo del sistema y cambie a su directorio de inicio.

En Linux o Mac:

    $ cd

  En Windows:

    C:\> cd %HOMEPATH%

2. Utilizando el símbolo del sistema, cree un directorio para su código llamado `web-service-gin`.

    $ mkdir web-service-gin
    $ cd web-service-gin

3. Cree un módulo en el que pueda gestionar las dependencias.

  Ejecute el comando `go mod init`, dándole la ruta del módulo en el que estará su código.

    $ go mod init example/web-service-gin
    go: creating new go.mod: module example/web-service-gin

Este comando crea un archivo go.mod en el que las dependencias que añada se enumerarán para su seguimiento. Para obtener más información sobre cómo nombrar un módulo con una ruta de módulo, consulte [Gestión de dependencias](https://go.dev/doc/modules/managing-dependencies#naming_module).

A continuación, diseñarás estructuras de datos para manejar datos.

## Crear los datos

Para simplificar el tutorial, los datos se almacenarán en memoria. Una API más típica interactuaría con una base de datos.

Ten en cuenta que almacenar los datos en memoria significa que el conjunto de álbumes se perderá cada vez que detengas el servidor, y se volverá a crear cuando lo inicies.

**Escribe el código**

1. Utilizando tu editor de texto, crea un archivo llamado main.go en el directorio web-service. Escribirás tu código Go en este archivo.
2. En main.go, en la parte superior del archivo, pega la siguiente declaración de paquete.

    package main

  Un programa independiente (a diferencia de una biblioteca) siempre se encuentra en el paquete main.

3. Debajo de la declaración del paquete, pega la siguiente declaración de una estructura de álbum. La utilizarás para almacenar los datos del álbum en memoria.

  Las etiquetas de estructura como json: "artist" especifican el nombre que debe tener un campo cuando el contenido de la estructura se serializa en JSON. Sin ellas, el JSON usaría los nombres de campo en mayúsculas del struct - un estilo no tan común en JSON.

    // album represents data about a record album.
    type album struct {
        ID     string  `json:"id"`
        Title  string  `json:"title"`
        Artist string  `json:"artist"`
        Price  float64 `json:"price"`
    }

4. Debajo de la declaración struct que acabas de añadir, pega el siguiente trozo de structs de álbumes que contienen datos que utilizarás para empezar.

`main.go`

    // albums slice to seed record album data.
    var albums = []album{
        {ID: "1", Title: "Blue Train", Artist: "John Coltrane", Price: 56.99},
        {ID: "2", Title: "Jeru", Artist: "Gerry Mulligan", Price: 17.99},
        {ID: "3", Title: "Sarah Vaughan and Clifford Brown", Artist: "Sarah Vaughan", Price: 39.99},
    }

A continuación, escribirás código para implementar tu primer endpoint.