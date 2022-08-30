# Retorno y gestión de un error

Gestionar errores es una característica esencial de código solido. En esta sección, añadirás un poco de código para retornar un error desde el módulo `greetings` después lo gestionaras en el módulo que realiza la llamada.

**Nota:** Este articulo es parte de los multiples tutoriales que inician con [Crear un módulo de Go](../2.1.Tutorial-Crear_un_modulo_de_go/README.md)

  1. En `greetings/greetings.go`, agrega el siguiente código.
  
  No tiene sentido retornar un saludo si no sabes a quien saludar. Retorna un error al que realiza la llamada si el nombre está vació. Copia el siguiente código dentro de greetings.go y guarda el archivo.

    package greetings

    import (
        "errors"
        "fmt"
    )

    // Hello returns a greeting for the named person.
    func Hello(name string) (string, error) {
        // If no name was given, return an error with a message.
        if name == "" {
            return "", errors.New("empty name")
        }

        // If a name was received, return a value that embeds the name
        // in a greeting message.
        message := fmt.Sprintf("Hi, %v. Welcome!", name)
        return message, nil
    }

  En esto código:

  - Cambias la función ahora esta retorna dos valores: un `string`y un `error`. La función que llama ahora chequeara el segundo valor para ver si ocurre algún error. (Toda función de Go puede retornar multiples valores. Para saber más, mira [Go Eficaz](https://go.dev/doc/effective_go.html#multiple-returns))
  - Importas el paquete `errors` de la libreria estandard de Go así que puedes usar la [función](https://pkg.go.dev/errors/#example-New) `errors.New`
  - Agregas un condicional `if` para comprobar si es una solicitud invalida (un string vacío donde debería ir el nombre) y retornar un error is la consulta es invalidad. La función `errors.New` retorna un error con un mensaje dentro.
  - Agregas `nil` (significa no error) como segundo valor en el retorno satisfactorio de la función. De esta forma la función de llamada puede ver que la función tuvo exito.

  2. En el archivo `hell/hello.go`, gestiona el error que ahora es retornado por la función `Hello`, junto al valor de no-error.

  Pega el siguiente código en `hello.go``

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

            // Request a greeting message.
            message, err := greetings.Hello("")
            // If an error was returned, print it to the console and
            // exit the program.
            if err != nil {
                log.Fatal(err)
            }

            // If no error was returned, print the returned message
            // to the console.
            fmt.Println(message)
        }

  En este código:

  - Se configura el [paquete](https://pkg.go.dev/log/) `log` para imprimir el error retornado con el prefijo ("greetings: ") en el mensaje de log, sin la información del tiempo o el archivo del error.
  - Asigna ambos valores retornados por `Hello`, incluido el `error`  las variables.
  - Se cambia el argumento del nombre de Gladys que se pasa a `Hello` por un string vacío para probar el código del error.
  - Se busca si el valor de `error` es distinto de  `nil`, en ese caso no tiene sentido continuar.
  - Se las funciones de la paquete `log` de la librería estándar de Go para retornar la información del error. Si se obtiene un error, se usa la función `Fata` del paquete `log` para imprimir el error y se detiene el programa.

  3. En la linea de comandos en el directorio `hello`, corre `hello.go` para confirmar que el codigo funciona.

  Ahora que pasas un nombre vacío, obtienes un error.

        $ go run .
        greetings: empty name
        exit status 1
    
Esto es el manejo común de errores en Go: Retornar un error como valor así al realizar la llamada puede chequearlo.

En el siguiente tutorial, usaras un segmento de Go para retornar un saludo al azar.

[Retornar un saludo aleatorio](../2.4.Tutorial-Retorna_un_saludo_aleatorio/README.md)