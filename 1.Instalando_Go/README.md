# Descargar e instalar

## 1. Descarga Go

<https://go.dev/dl/>

**Nota:** Por defecto, el comando go descarga y autentica los módulos usando el módulo `Go mirror` y `Go checksum database` dirigidos por Google

## 2. Instalar Go

### Linux

1. **Remueva cualquier instalación previa de Go** eliminando el directorio `/usr/local/go` (si este existe), después extraiga el archivo que descargaste dentro de `/usr/local`, creando un nuevo árbol de directorios en `/usr/local/go`

        rm -rf /usr/local/go && tar -C /usr/local -xzf go1.19.linux-amd64.tar.gz

2. Agregue `/usr/local/go/bin` a la variable de entorno `PATH`.

Puede hacer esto agregando la siguiente linea a `$HOME/.profile` o `/etc/profile` (para una instalación en todo el sistema):

        export PATH=$PATH:/usr/local/go/bin

**Nota:** Los cambios realizados pueden no aplicarse hasta la próxima vez que inicie su computadora. Para aplicar los cambios inmediatamente, solo corra el comando directamente en la shell o ejecútelo desde el perfil usando un comando como `source $HOME/.profile`.

3. Verifique que haya instalado Go abriendo una terminal y escribiendo el siguiente comando:

        go version

4. Confirme que el comando imprima la versión de Go instalada.

### Mac

1. Abra el paquete  que descargó y siga las instrucciones para instalar Go.

El paquete instala la distribución de Go en `/usr/local/go`. El paquete debería poner el directorio `/usr/local/go/bin` en su variable de entorno `PATH`. Debería reiniciar cada sesión de terminal para que tome efecto.

2. Verifique que se haya instalado Go abriendo una terminal de comando y escribiendo el siguiente comando:

        $ go version

3. Confirme que el comando imprima la versión instalada de Go

### Windows

Abra el archivo MSI que descargó y siga las instrucciones para instalar Go.

Por defecto, el instalador instalara Go en `Program Files` o `Program Files (x86)`. Puede cambiar la ubicación si lo necesita. Después de instalar, usted necesitara cerrar y volver a abrir cualquier consola de comandos abierta para que los cambios de entorno realizados por el instalador tomen efecto en la consola de comandos.

2. Verifique que haya instalado Go
    1. En **Windows**, de click en el menú **Inicio**
    2. En la caja de búsqueda del menú, escriba `cmd`, después presione la tecla `Enter`.
    3. En la ventana de consola de comando que aparece, escriba el siguiente comando:

            $ go version

    4. Confirme que el comando imprima la versión de Go instalada.

## 3. Go code.

Está listo! Visite el [tutorial Primeros Pasos](../1.Tutorial-Primeros_Pasos/README.md) para escribir algo código simple de Go. Le tomara al rededor de 10 minutos completarlo.
