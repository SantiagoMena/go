# Agrega un test

Ahora que has conseguido que tu código sea estable (muy bien hecho, por cierto), añade una prueba. Probar tu código durante el desarrollo puede exponer errores que encuentran su camino a medida que haces cambios. En este tema, añades una prueba para la función `Hello`.

**Nota:** Este articulo es parte de los multiples tutoriales que inician con [Crear un módulo de Go](../2.1.Tutorial-Crear_un_modulo_de_go/README.md)

El soporte integrado de Go para pruebas unitarias facilita la realización de pruebas sobre la marcha. En concreto, utilizando convenciones de nomenclatura, el paquete de pruebas de Go y el comando `go test`, puedes escribir y ejecutar pruebas rápidamente.

1. En el directorio greetings, cree un archivo llamado greetings_test.go.
Terminar el nombre de un archivo con `_test.go` indica al comando `go test` que este archivo contiene funciones de prueba.

2. En `greetings_test.go`, pega el siguiente código y guarda el archivo.

`greetings_test.go`

    package greetings

    import (
        "testing"
        "regexp"
    )

    // TestHelloName calls greetings.Hello with a name, checking
    // for a valid return value.
    func TestHelloName(t *testing.T) {
        name := "Gladys"
        want := regexp.MustCompile(`\b`+name+`\b`)
        msg, err := Hello("Gladys")
        if !want.MatchString(msg) || err != nil {
            t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`, msg, err, want)
        }
    }

    // TestHelloEmpty calls greetings.Hello with an empty string,
    // checking for an error.
    func TestHelloEmpty(t *testing.T) {
        msg, err := Hello("")
        if msg != "" || err == nil {
            t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
        }
    }

En este código, usted:

- Implementa funciones de prueba en el mismo paquete que el código que estás probando.
- Crear dos funciones de prueba para probar la función `greetings.Hello`. Los nombres de las funciones de prueba tienen la forma `TestName`, donde Name dice algo sobre la prueba específica. Además, las funciones de prueba toman como parámetro un puntero al tipo testing.T del paquete de pruebas. Utilice los métodos de este parámetro para informar y registrar su prueba.
- Implemente dos pruebas:
  - `TestHelloName` llama a la función Hello, pasando un valor de nombre con el que la función debería poder devolver un mensaje de respuesta válido. Si la llamada devuelve un error o un mensaje de respuesta inesperado (uno que no incluya el nombre que pasaste), utiliza el método `Fatalf` del parámetro t para imprimir un mensaje en la consola y finalizar la ejecución.
  - TestHelloEmpty llama a la función Hello con una cadena vacía. Esta prueba está diseñada para confirmar que su manejo de errores funciona. Si la llamada devuelve una cadena no vacía o ningún error, se utiliza el método `Fatalf` del parámetro t para imprimir un mensaje en la consola y finalizar la ejecución.

3. En la línea de comandos del directorio `greetings`, ejecute el comando `go test` para ejecutar la prueba.
El comando `go test` ejecuta funciones de prueba (cuyos nombres comienzan por Test) en archivos de prueba (cuyos nombres terminan por _test.go). Puede añadir el indicador -v para obtener una salida detallada que enumere todas las pruebas y sus resultados.

  Las pruebas deben pasar.
    $ go test
    PASS
    ok      example.com/greetings   0.364s

    $ go test -v
    === RUN   TestHelloName
    --- PASS: TestHelloName (0.00s)
    === RUN   TestHelloEmpty
    --- PASS: TestHelloEmpty (0.00s)
    PASS
    ok      example.com/greetings   0.372s

4. Rompa la función `greetings.Hello` para ver una prueba fallida.
La función de prueba `TestHelloName` comprueba el valor de retorno del nombre especificado como parámetro de la función Hello. Para ver un resultado de prueba fallido, cambie la función `greetings.Hello` para que ya no incluya el nombre.

En `greetings/greetings.go`, pegue el siguiente código en lugar de la función Hola. Observe que las líneas resaltadas cambian el valor que devuelve la función, como si el argumento nombre se hubiera eliminado accidentalmente.

    // Hello returns a greeting for the named person.
    func Hello(name string) (string, error) {
        // If no name was given, return an error with a message.
        if name == "" {
            return name, errors.New("empty name")
        }
        // Create a message using a random format.
        // message := fmt.Sprintf(randomFormat(), name)
        message := fmt.Sprint(randomFormat())
        return message, nil
    }

5. En la línea de comandos del directorio greetings, ejecute `go test` para ejecutar la prueba.
Esta vez, ejecute `go test` sin el indicador -v. La salida incluirá los resultados de sólo las pruebas que fallaron, lo que puede ser útil cuando se tienen muchas pruebas. 

La prueba `TestHelloName` debería fallar `-- TestHelloEmpty` sigue pasando.

    $ go test
    --- FAIL: TestHelloName (0.00s)
        greetings_test.go:15: Hello("Gladys") = "Hail, %v! Well met!", <nil>, want match for `\bGladys\b`, nil
    FAIL
    exit status 1
    FAIL    example.com/greetings   0.182s

En el siguiente (y último) tema, verás cómo compilar e instalar tu código para ejecutarlo localmente.