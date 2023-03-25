# Retorna un saludo aleatorio

En esta sección, cambiarás tu código para que en lugar de devolver un único saludo cada vez, devuelva uno de varios mensajes de saludo predefinidos.

**Nota:** Este articulo es parte de los multiples tutoriales que inician con [Crear un módulo de Go](../2.1.Tutorial-Crear_un_modulo_de_go/README.md)

Para ello, utilizarás una Go slice. Un slice es como un array, excepto que su tamaño cambia dinámicamente a medida que añades y eliminas elementos. El slice es uno de los tipos más útiles de Go.

Añadirás una pequeña porción para contener tres mensajes de saludo, y luego harás que tu código devuelva uno de los mensajes aleatoriamente. Para más información sobre slices, vea [Go slices](https://blog.golang.org/slices-intro) en el blog de Go.

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

    // init sets initial values for variables used in the function.
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

        // Return a randomly selected message format by specifying
        // a random index for the slice of formats.
        return formats[rand.Intn(len(formats))]
    }

En este código:

Añade una función randomFormat que devuelve un formato seleccionado al azar para un mensaje de saludo. Tenga en cuenta que randomFormat comienza con una letra minúscula, por lo que es accesible sólo para el código en su propio paquete (en otras palabras, no se exporta).
En randomFormat, declara un slice de formatos con tres formatos de mensaje. Al declarar un slice, omite su tamaño entre paréntesis, así []cadena. Esto le dice a Go que el tamaño de la matriz subyacente a la rebanada se puede cambiar dinámicamente.
Usa el paquete math/rand para generar un número aleatorio para seleccionar un elemento de la porción.
Añada una función init para sembrar el paquete rand con la hora actual. Go ejecuta las funciones init automáticamente al inicio del programa, después de que las variables globales hayan sido inicializadas. Para más información sobre las funciones init, vea Go Efectivo.
En Hello, llama a la función randomFormat para obtener un formato para el mensaje que devolverás, luego usa el formato y el valor del nombre juntos para crear el mensaje.
Devuelve el mensaje (o un error) como hiciste antes.

Traducción realizada con la versión gratuita del traductor www.DeepL.com/Translator