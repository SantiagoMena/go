# Retorna un saludo aleatorio

En los últimos cambios que harás en el código de tu módulo, añadirás soporte para obtener saludos de múltiples personas en una petición. En otras palabras, manejarás una entrada de múltiples valores, y luego emparejarás los valores de esa entrada con una salida de múltiples valores. Para ello, tendrás que pasar un conjunto de nombres a una función que pueda devolver un saludo para cada uno de ellos.

**Nota:** Este articulo es parte de los multiples tutoriales que inician con [Crear un módulo de Go](../2.1.Tutorial-Crear_un_modulo_de_go/README.md)

Pero hay un problema. Cambiar el parámetro de la función `hello` de un solo nombre a un conjunto de nombres cambiaría la firma de la función. Si ya has publicado el módulo `example.com/greetings` y los usuarios ya han escrito código llamando a Hello, ese cambio rompería sus programas.

En esta situación, una mejor opción es escribir una nueva función con un nombre diferente. La nueva función tomará múltiples parámetros. Esto preserva la antigua función para compatibilidad con versiones anteriores

1. En `greetings/greetings.go`, cambia el código para que tenga el siguiente aspecto.

`greetings/greetings.go`

    package greetings

    import (
        "errors"
        "fmt"
        "math/rand"
        "time"
    )

    // Hello returns a greeting for the named person.
    func Hello(name string) (string, error) {
        // If no name was given, return an error with a message.
        if name == "" {
            return name, errors.New("empty name")
        }
        // Create a message using a random format.
        message := fmt.Sprintf(randomFormat(), name)
        return message, nil
    }

    // Hellos returns a map that associates each of the named people
    // with a greeting message.
    func Hellos(names []string) (map[string]string, error) {
        // A map to associate names with messages.
        messages := make(map[string]string)
        // Loop through the received slice of names, calling
        // the Hello function to get a message for each name.
        for _, name := range names {
            message, err := Hello(name)
            if err != nil {
                return nil, err
            }
            // In the map, associate the retrieved message with
            // the name.
            messages[name] = message
        }
        return messages, nil
    }

    // Init sets initial values for variables used in the function.
    func init() {
        rand.Seed(time.Now().UnixNano())
    }

    // randomFormat returns one of a set of greeting messages. The returned
    // message is selected at random.
    func randomFormat() string {
        // A slice of message formats.
        formats := []string{
            "Hi, %v. Welcome!",
            "Great to see you, %v!",
            "Hail, %v! Well met!",
        }

        // Return one of the message formats selected at random.
        return formats[rand.Intn(len(formats))]
    }

En este código:

Añades una función `Hellos` cuyo parámetro es una porción de nombres en lugar de un único nombre. Además, cambia uno de sus tipos de retorno de una cadena a un mapa para que pueda devolver nombres asignados a mensajes de saludo.
Haz que la nueva función Hellos llame a la función Hello existente. Esto ayuda a reducir la duplicación a la vez que deja ambas funciones en su lugar.
Cree un mapa de mensajes para asociar cada uno de los nombres recibidos (como clave) con un mensaje generado (como valor). En Go, se inicializa un mapa con la siguiente sintaxis: `make(map[key-type]value-type)`. La función Hellos devuelve este mapa a quien lo llama. Para más información sobre mapas, consulta [Go maps in action]('https://go.dev/blog/maps') en el blog de Go.
Recorre en bucle los nombres que recibió tu función, comprobando que cada uno tenga un valor no vacío, y luego asocia un mensaje a cada uno. En este bucle for, range devuelve dos valores: el índice del elemento actual en el bucle y una copia del valor del elemento. No necesita el índice, por lo que utiliza el identificador Go blank (un guión bajo) para ignorarlo. Para más información, vea [El identificador en blanco]('https://go.dev/doc/effective_go#blank') en Go efectivo.

2. En su código de llamada `hello/hello.go`, pase una porción de nombres, luego imprima el contenido del mapa de `names/messages` que obtiene de vuelta.
En hello.go, cambia tu código para que se parezca a lo siguiente.

`hello/hello.go`

    package main

    import (
        "fmt"
        "log"

        "example.com/greetings"
    )

    func main() {
        // Set properties of the predefined Logger, including
        // the log entry prefix and a flag to disable printing
        // the time, source file, and line number.
        log.SetPrefix("greetings: ")
        log.SetFlags(0)

        // A slice of names.
        names := []string{"Gladys", "Samantha", "Darrin"}

        // Request greeting messages for the names.
        messages, err := greetings.Hellos(names)
        if err != nil {
            log.Fatal(err)
        }
        // If no error was returned, print the returned map of
        // messages to the console.
        fmt.Println(messages)
    }

Con estos cambios:

- Crear una variable names como tipo slice que contenga tres nombres.
- Pasar la variable names como argumento a la función Hellos.

3. En la línea de comandos, cambie al directorio que contiene hello/hello.go y, a continuación, utilice go run para confirmar que el código funciona.
La salida debería ser una representación de cadena del mapa que asocia nombres con mensajes, algo como lo siguiente:

`commnad line:`

    $ go run .
    map[Darrin:Hail, Darrin! Well met! Gladys:Hi, Gladys. Welcome! Samantha:Hail, Samantha! Well met!]

Este tema introdujo mapas para representar pares nombre/valor. También introdujo la idea de preservar la compatibilidad con versiones anteriores mediante la implementación de una nueva función para la funcionalidad nueva o modificada en un módulo. Para más información sobre compatibilidad con versiones anteriores, consulta [Mantener la compatibilidad de tus módulos]('https://go.dev/blog/module-compatibility').

A continuación, utilizarás las funciones integradas de Go para crear una prueba unitaria para tu código.