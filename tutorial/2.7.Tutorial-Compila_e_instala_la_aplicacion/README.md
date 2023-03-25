# Compila e instala la aplicación

En este último tema, aprenderás un par de comandos go nuevos. Mientras que el comando go run es un atajo útil para compilar y ejecutar un programa cuando estás haciendo cambios frecuentes, no genera un ejecutable binario.

Este tema introduce dos comandos adicionales para construir código:

- El [comando]('https://go.dev/cmd/go/#hdr-Compile_packages_and_dependencies') `go build` compila los paquetes, junto con sus dependencias, pero no instala los resultados.
- El [comando]('https://go.dev/ref/mod#go-install') `go install` compila e instala los paquetes.

**Nota:** Este articulo es parte de los multiples tutoriales que inician con [Crear un módulo de Go](../2.1.Tutorial-Crear_un_modulo_de_go/README.md)

1. Desde la línea de comandos en el directorio `hello`, ejecute el comando go build para compilar el código en un ejecutable.

`/hello`

    $ go build

2. Desde la línea de comandos del directorio hello, ejecute el nuevo ejecutable hello para confirmar que el código funciona.
Ten en cuenta que el resultado puede variar dependiendo de si has cambiado el código de greetings.go después de probarlo.

En Linux o Mac:

    $ ./hello
    map[Darrin:Great to see you, Darrin! Gladys:Hail, Gladys! Well met! Samantha:Hail, Samantha! Well met!]

En windows

    $ hello.exe
    map[Darrin:Great to see you, Darrin! Gladys:Hail, Gladys! Well met! Samantha:Hail, Samantha! Well met!]

Has compilado la aplicación en un ejecutable para poder ejecutarla. Pero para ejecutarla actualmente, tu prompt necesita estar en el directorio del ejecutable, o especificar la ruta del ejecutable.

A continuación, instalarás el ejecutable para poder ejecutarlo sin especificar su ruta.

3. Descubre la ruta de instalación de Go, donde el comando go instalará el paquete actual.

Puede descubrir la ruta de instalación ejecutando el comando go list, como en el siguiente ejemplo:

    $ go list -f '{{.Target}}'

Por ejemplo, la salida del comando puede decir `/home/gopher/bin/hello`, lo que significa que los binarios están instalados en `/home/gopher/bin`. Necesitará este directorio de instalación en el siguiente paso.

4. Añada el directorio de instalación de Go a la ruta de shell de su sistema.
De esta forma, podrás ejecutar el ejecutable de tu programa sin especificar dónde se encuentra el ejecutable.

En Linux o Mac, ejecuta el siguiente comando:

    $ export PATH=$PATH:/path/to/your/install/directory

On Windows, run the following command:

    $ set PATH=%PATH%;C:\path\to\your\install\directory

Como alternativa, si ya tiene un directorio como `$HOME/bin` en su ruta de shell y desea instalar sus programas Go allí, puede cambiar el objetivo de la instalación estableciendo la variable `GOBIN` usando el comando go env:

    $ go env -w GOBIN=/path/to/your/bin

o

    $ go env -w GOBIN=C:\path\to\your\bin

5- Una vez que haya actualizado la ruta del shell, ejecute el comando go install para compilar e instalar el paquete.

    $ go install

Ejecute su aplicación simplemente escribiendo su nombre. Para hacer esto interesante, abra un nuevo símbolo del sistema y ejecute el nombre ejecutable hola en algún otro directorio.

    $ hello
    map[Darrin:Hail, Darrin! Well met! Gladys:Great to see you, Gladys! Samantha:Hail, Samantha! Well met!]

Con esto terminamos este tutorial de Go.
