# Llama tu código desde otro módulo

In la [sección anterior](../2.1.Tutorial-Crear_un_modulo_de_go/README.md), creaste el módulo `greetings`. In esta sección, escribirás código para realizar llamadas a la función `Hello`en el modulo que escribiste. Escribirás código que puedes ejecutar como una aplicación y que llamará al código en el módulo `greetings`.

**Nota:** Este articulo es parte de los multiples tutoriales que inician con [Crear un módulo de Go](../2.1.Tutorial-Crear_un_modulo_de_go/README.md)

1. Crea un directorio `hello` para el código fuente de tu módulo de Go. Este es dónde escribirás la llamada.

Después de crear este directorio, deberías tener ambos directorios `hello` y `greetings`en el mismo nivel de jerarquía, algo así:

        <home>/
        |-- greetings/
        |-- hello/

Por ejemplo, si tu terminal de comando está en el directorio `greetings` deberías usar los siguientes comandos:

        cd ..
        mkdir hello
        cd hello

2. Habilitar el seguimiento de dependencias para el código que estás a punto de escribir.

Para habilitar el seguimiento de dependencias para tu código, corre el [comando](https://go.dev/ref/mod#go-mod-init) `go mod init`, proporcionando el nombre del modulo en el que tu código va a estar.

Para el propósito de este tutorial, usa `example.com/hello` para el directorio del módulo.

        $ go mod init example.com/hello
        go: creating new go.mod: module example.com/hello

3. En tu editor de tecto, en el directorio `hello`, crea un archivo para escribir tu código y nombralo `hello.go`.
4. Escribe el código para llamar a la función `Hello`, despues imprime el valor que retorna la función.

Para hacer esto, pega el siguiente código dentro de `hello.go`.

En este código haz:

- Declarado un paquete `main`. En Go, el código ejecutado como una aplicación debe ir en un paquete `main`.
- Importado dos paquetes: `example.com/greetings` y el [paquete](https://pkg.go.dev/fmt/) `fmt`. Esto le da acceso a tu código a las funciones de esos paquetes. Importando `example.com/greetings` (el paquete contenido en el módulo que creaste antes) te da acceso a la función `Hello`. Además importaste `fmt`, con las funciones para manipular entradas y salidas de texto (como imprimir texto en la consola).
- Obtuviste un saludo llamando a la función `Hello` del paquete `greetings`

5. Edita el módulo `example.com/hello/` para usar tu módulo local `example.com/greetings`

Para el uso en producción, debes publicar el repositorio de tu módulo `example.com/greetings` (con la ruta del módulo que refleje la localización de su publicación), donde la herramientas de Go puedan encontrarlo y descargarlo. Por ahora, como no haz publicado el código aun, necesitas adaptar el módulo `example.com/hello` así puede encontrar el código de `example.com/hello` en tu sistema local de archivos.

Para hacer esto, usa el [comando](https://go.dev/ref/mod#go-mod-edit) `go mod edit` para editar el módulo `example.com/hello` para redirigir las herramientas de Go de ese directorio del módulo (donde el modulo no está) al directorio local (donde se encuentra).

  1. Desde la terminal de comandos en el directorio `hello` corre el siguiente comando:

                $ go mod edit -replace example.com/greetings=../greetings

El comando especifica que `example.com/greetings` sera reemplazado con `../greetings` para el propósito de localizar la dependencia. Después de correr el comando, el archivo `go.mod` en el directorio `hello` debería incluir una [directiva](https://go.dev/doc/modules/gomod-ref#replace) `replace`.

        module example.com/hello

        go 1.16

        replace example.com/greetings => ../greetings

  2. Desde la terminal de comandos en el directorio `hello`, corre el [comando](https://go.dev/ref/mod#go-mod-tidy) para sincronizar las dependencias del módulo `example.com/hello`, añadiendo los requisitos del código, pero aún no restreándolos en el módulo.

                $ go mod tidy
                go: found example.com/greetings in example.com/greetings v0.0.0-00010101000000-000000000000

  3. Después que el comando termine, el archivo `go.mod` del módulo `example.com/hello` debería verse así:

                module example.com/hello

                go 1.16

                replace example.com/greetings => ../greetings

                require example.com/greetings v0.0.0-00010101000000-000000000000

  El comando encuentra el código local en el directorio `greetings`, entonces añade la [directiva/política](https://go.dev/doc/modules/gomod-ref#require) `require` para especificar que `example.com/hello` requiere `example.com/greetings`.  Haz creado esta dependencia cuando se importó el paquete `greetings` dentro de `hello.go`.

  El número siguiente del directorio del módulo es un nímero de pseudo-version -- un número generado que es usado en lugar de un número de versión semantico (Que el módulo aun no tiene).

  Para referenciar un módulo *publicado*, un archivo `go.mod` normalmente omitiría la directiva/política `replace` y usaría una directiva `require` con una versión etiquetada al final.

        require example.com/greetings v1.1.0

Para saber más sobre números de versiones, mira [Numerado de versiones de módulos](https://go.dev/doc/modules/version-numbers).

6. En la terminal de comandos en el directorio `hello`, corre tu código para confirmar si funciona.

        $ go run .
        Hi, Gladys. Welcome!

Felicitaciones! Haz escrito dos módulos funcionales.
En el siguiente tutorial, agregaras gestiones de errores.

[Retorna y gestiona de un error](../2.3.Tutorial-Retorna_y_gestiona_un_error/README.md)