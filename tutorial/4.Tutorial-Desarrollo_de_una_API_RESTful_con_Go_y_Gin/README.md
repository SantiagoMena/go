# Tutorial: Desarrollo de una API RESTful con Go y Gin

Tabla de contenido

- [x] [Requisitos previos](#requisitos-previos)
- [x] [Diseñar los endpoints de la API](#diseñar-los-endpoints-de-la-api)
- [x] [Cree una carpeta para su código](#cree-una-carpeta-para-su-código)
- [x] [Crear los datos](#crear-los-datos)
- [x] [Escribir un handler para devolver todos los elementos](#escribir-un-handler-para-devolver-todos-los-elementos)
- [x] [Escribir un handler para añadir un nuevo elemento](#escribir-un-handler-para-añadir-un-nuevo-elemento)
- [x] [Escribir un handler para devolver un elemento específico](#escribir-un-handler-para-devolver-un-elemento-específico)
- [x] [Conclusión](#conclusión)
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

## Escribir un handler para devolver todos los elementos

Cuando el cliente hace una petición en GET /albums, quieres devolver todos los álbumes como JSON.
Para ello, escribirás lo siguiente:

- Lógica para preparar una respuesta
- Código para mapear la ruta de la petición a tu lógica

Ten en cuenta que esto es al revés de cómo se ejecutarán en tiempo de ejecución, pero primero estás añadiendo dependencias y luego el código que depende de ellas.

**Escribe el código**

1. Debajo del código struct que añadiste en la sección anterior, pega el siguiente código para obtener la lista de álbumes.

  Esta función getAlbums crea JSON a partir de la rebanada de structs de álbumes, escribiendo el JSON en la respuesta.

    // getAlbums responds with the list of all albums as JSON.
    func getAlbums(c *gin.Context) {
        c.IndentedJSON(http.StatusOK, albums)
    }

En este código

- Escribe una función getAlbums que toma un parámetro gin.Context. Tenga en cuenta que podría haber dado a esta función cualquier nombre - ni Gin ni Go requieren un formato de nombre de función en particular.

- gin.Context es la parte más importante de Gin. Lleva los detalles de la solicitud, valida y serializa JSON, y más. (A pesar del nombre similar, es diferente del paquete context incorporado de Go).

- Llama a Context.IndentedJSON para serializar la estructura en JSON y añadirla a la respuesta.

  El primer argumento de la función es el código de estado HTTP que quieres enviar al cliente. Aquí, estás pasando la constante StatusOK del paquete net/http para indicar 200 OK.

  Tenga en cuenta que puede sustituir Context.IndentedJSON por una llamada a Context.JSON para enviar un JSON más compacto. En la práctica, la forma indentada es mucho más fácil de trabajar cuando se depura y la diferencia de tamaño suele ser pequeña.

2. Cerca de la parte superior de main.go, justo debajo de la declaración de slice albums, pega el código de abajo para asignar la función handler a una ruta endpoint.

  Esto establece una asociación en la que getAlbums maneja las solicitudes a la ruta del punto final /albums.

    func main() {
        router := gin.Default()
        router.GET("/albums", getAlbums)

        router.Run("localhost:8080")
    }

En este código, usted:

- Inicializa un enrutador Gin utilizando Default.

- Usar la función GET para asociar el método HTTP GET y la ruta /albums con una función manejadora.

  Ten en cuenta que estás pasando el nombre de la función getAlbums. Esto es diferente de pasar el resultado de la función, lo que harías pasando getAlbums() (observa el paréntesis).

- Utilice la función Ejecutar para conectar el enrutador a un http.Server e iniciar el servidor.

3. Cerca de la parte superior de main.go, justo debajo de la declaración de paquetes, importa los paquetes que necesitarás para soportar el código que acabas de escribir.

Las primeras líneas de código deben tener este aspecto:

    package main

    import (
        "net/http"

        "github.com/gin-gonic/gin"
    )

4. Guarda main.go.

**Ejecutar el código**

1. Comienza a seguir el módulo Gin como una dependencia.

  En la línea de comandos, utiliza go get para añadir el módulo github.com/gin-gonic/gin como una dependencia para tu módulo. Usa un argumento de punto para significar "obtener dependencias para el código en el directorio actual".

    $ go get .
    go get: added github.com/gin-gonic/gin v1.7.2

2- Go ha resuelto y descargado esta dependencia para satisfacer la declaración de importación que ha añadido en el paso anterior.

  Desde la línea de comandos en el directorio que contiene main.go, ejecuta el código. Usa un argumento de punto para decir "ejecuta el código en el directorio actual".

    $ go run .

  Una vez que el código se está ejecutando, tienes un servidor HTTP en ejecución al que puedes enviar peticiones.

3. Desde una nueva ventana de línea de comandos, utiliza curl para hacer una petición a tu servicio web en ejecución.

    $ curl http://localhost:8080/albums

  El comando debería mostrar los datos con los que ha sembrado el servicio.

    [
            {
                    "id": "1",
                    "title": "Blue Train",
                    "artist": "John Coltrane",
                    "price": 56.99
            },
            {
                    "id": "2",
                    "title": "Jeru",
                    "artist": "Gerry Mulligan",
                    "price": 17.99
            },
            {
                    "id": "3",
                    "title": "Sarah Vaughan and Clifford Brown",
                    "artist": "Sarah Vaughan",
                    "price": 39.99
            }
    ]

¡Has iniciado una API! En la siguiente sección, crearás otro endpoint con código para gestionar una solicitud POST para añadir un elemento.

## Escribir un handler para añadir un nuevo elemento

Cuando el cliente hace una petición POST en /albums, quieres añadir el álbum descrito en el cuerpo de la petición a los datos de álbumes existentes.

Para ello, escribirás lo siguiente:

- Lógica para añadir el nuevo álbum a la lista existente.
- Un poco de código para dirigir la petición POST a tu lógica.

**Escribe el código**

1. Añade código para añadir los datos de los álbumes a la lista de álbumes.

  En algún lugar después de las declaraciones de importación, pegue el siguiente código. (El final del archivo es un buen lugar para este código, pero Go no impone el orden en el que declaras las funciones).

    // postAlbums adds an album from JSON received in the request body.
    func postAlbums(c *gin.Context) {
        var newAlbum album

        // Call BindJSON to bind the received JSON to
        // newAlbum.
        if err := c.BindJSON(&newAlbum); err != nil {
            return
        }

        // Add the new album to the slice.
        albums = append(albums, newAlbum)
        c.IndentedJSON(http.StatusCreated, newAlbum)
    }

En este código

- Usas Context.BindJSON para enlazar el cuerpo de la petición a newAlbum.
- Añade la estructura de álbumes inicializada a partir del JSON a la porción de álbumes.
- Añade un código de estado 201 a la respuesta, junto con el JSON que representa el álbum añadido.

1. Cambia tu función principal para que incluya la función router.POST,

  como en lo siguiente.

    func main() {
        router := gin.Default()
        router.GET("/albums", getAlbums)
        router.POST("/albums", postAlbums)

        router.Run("localhost:8080")
    }

En este código

- Asocias el método POST en la ruta /albums con la función postAlbums.

  Con Gin, puedes asociar un handler con una combinación de método HTTP y ruta. De esta forma, puedes enrutar por separado las peticiones enviadas a una única ruta en función del método que esté utilizando el cliente.

**Run the code**

1. Si el servidor sigue funcionando desde la última sección, deténgalo.
2. Desde la línea de comandos en el directorio que contiene main.go, ejecute el código.

    $ go run .

3. Desde una ventana de línea de comandos diferente, 

utilice curl para realizar una petición a su servicio web en ejecución.

    $ curl http://localhost:8080/albums \
        --include \
        --header "Content-Type: application/json" \
        --request "POST" \
        --data '{"id": "4","title": "The Modern Sound of Betty Carter","artist": "Betty Carter","price": 49.99}'

  El comando debería mostrar las cabeceras y el JSON del álbum añadido.

    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Wed, 02 Jun 2021 00:34:12 GMT
    Content-Length: 116

    {
        "id": "4",
        "title": "The Modern Sound of Betty Carter",
        "artist": "Betty Carter",
        "price": 49.99
    }

4. Como en la sección anterior, utilice curl para recuperar la lista completa de álbumes, 

que puede utilizar para confirmar que se ha añadido el nuevo álbum.

    $ curl http://localhost:8080/albums \
        --header "Content-Type: application/json" \
        --request "GET"

El comando debería mostrar la lista de álbumes.

    [
            {
                    "id": "1",
                    "title": "Blue Train",
                    "artist": "John Coltrane",
                    "price": 56.99
            },
            {
                    "id": "2",
                    "title": "Jeru",
                    "artist": "Gerry Mulligan",
                    "price": 17.99
            },
            {
                    "id": "3",
                    "title": "Sarah Vaughan and Clifford Brown",
                    "artist": "Sarah Vaughan",
                    "price": 39.99
            },
            {
                    "id": "4",
                    "title": "The Modern Sound of Betty Carter",
                    "artist": "Betty Carter",
                    "price": 49.99
            }
    ]

En la siguiente sección, añadirás código para gestionar un GET para un elemento específico.

## Escribir un handler para devolver un elemento específico

Cuando el cliente hace una petición GET /albums/[id], quieres devolver el álbum cuyo ID coincide con el parámetro de ruta id.

Para ello

- Añadir lógica para recuperar el álbum solicitado.
- Asignar la ruta a la lógica.

**Escribir el código**

1. Debajo de la función postAlbums que has añadido en la sección anterior, pega el siguiente código para recuperar un álbum específico.

  Esta función getAlbumByID extraerá el ID de la ruta solicitada y localizará un álbum que coincida.

    // getAlbumByID locates the album whose ID value matches the id
    // parameter sent by the client, then returns that album as a response.
    func getAlbumByID(c *gin.Context) {
        id := c.Param("id")

        // Loop over the list of albums, looking for
        // an album whose ID value matches the parameter.
        for _, a := range albums {
            if a.ID == id {
                c.IndentedJSON(http.StatusOK, a)
                return
            }
        }
        c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
    }

En este código, Ud:

- Utiliza Context.Param para recuperar el parámetro id path de la URL. Cuando asigne este controlador a una ruta, incluirá un marcador de posición para el parámetro en la ruta.

- Recorre los structs de álbumes en el slice, buscando uno cuyo valor del campo ID coincida con el valor del parámetro id. Si lo encuentras, serializa esa estructura de álbum a JSON y devuélvela como respuesta con un código HTTP 200 OK.

  Como se mencionó anteriormente, un servicio del mundo real probablemente utilizaría una consulta a la base de datos para realizar esta búsqueda.

- Devuelve un error HTTP 404 con http.StatusNotFound si no se encuentra el álbum.

2. Por último, cambia tu main para que incluya una nueva llamada a router.GET, donde la ruta es ahora /albums/:id,

como se muestra en el siguiente ejemplo.

    func main() {
        router := gin.Default()
        router.GET("/albums", getAlbums)
        router.GET("/albums/:id", getAlbumByID)
        router.POST("/albums", postAlbums)

        router.Run("localhost:8080")
    }

En este código, Ud:

- Asocias la ruta /albums/:id con la función getAlbumByID. En Gin, los dos puntos que preceden a un elemento de la ruta significan que el elemento es un parámetro de la ruta.

**Ejecute el código**

1. Si el servidor sigue ejecutándose desde la última sección, deténgalo.
2. Desde la línea de comandos en el directorio que contiene main.go, 

ejecute el código para iniciar el servidor.

    $ go run .

3. Desde una ventana de línea de comandos diferente, 

utilice curl para realizar una petición a su servicio web en ejecución.

    $ curl http://localhost:8080/albums/2

El comando debería mostrar JSON para el álbum cuyo ID hayas utilizado. Si no se encuentra el álbum, obtendrás JSON con un mensaje de error.

    {
        "id": "2",
        "title": "Jeru",
        "artist": "Gerry Mulligan",
        "price": 17.99
    }

## Conclusión

¡Enhorabuena! Acabas de usar Go y Gin para escribir un simple servicio web RESTful.

Próximos temas sugeridos:

Si eres nuevo en Go, encontrarás útiles mejores prácticas descritas en [Effective Go](https://go.dev/doc/effective_go) y [How to write Go code](https://go.dev/doc/code).
El [Tour Go](https://go.dev/tour/welcome/1) es una excelente introducción paso a paso a los fundamentos de Go.
Para más información sobre Gin, consulte la [documentación del paquete Gin Web Framework](https://pkg.go.dev/github.com/gin-gonic/gin) o la [documentación de Gin Web Framework](https://gin-gonic.com/docs/).