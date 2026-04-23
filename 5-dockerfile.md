# Dockerfile
Un Dockerfile es un archivo de texto plano que contiene una serie de instrucciones que Docker utiliza para construir una imagen de contenedor Docker. Este conjunto de instrucciones define cómo se debe configurar y construir una imagen de contenedor, incluyendo qué sistema operativo base usar, qué software instalar, qué archivos copiar en el contenedor y cómo configurar el entorno de ejecución.
 ![Dockerfile](img/relacion.PNG)


Tradicionalmente, el archivo docker no tiene extensión. Simplemente se denomina Dockerfile sin ningún tipo de extensión. Adicionalmente, los Dockerfiles pueden ser creados usando la extensión .dockerfile. Esto se utiliza cuando hay una necesidad de almacenar múltiples archivos docker en un solo directorio.
Las instrucciones en un Dockerfile son simples y están diseñadas para ser leídas y comprendidas fácilmente. 

### FROM 
Especifica la imagen base que se utilizará como punto de partida para construir la nueva imagen de Docker. Por ejemplo, FROM nginx:alpine significa que la nueva imagen se basará en la imagen oficial de Nginx que está etiquetada como "alpine". Al utilizar una imagen base existente, se heredan todas las configuraciones y software incluidos en esa imagen, lo que facilita la creación de nuevas imágenes sin tener que empezar desde cero.

### RUN
Ejecuta comandos en el contenedor durante el proceso de construcción de la imagen. Puedes usar esta instrucción para instalar software, configurar el entorno, o realizar cualquier otra tarea necesaria para preparar la imagen. Por ejemplo, RUN apt-get update && apt-get install -y <paquete> instalará un paquete utilizando el administrador de paquetes apt en una imagen basada en Ubuntu.

### COPY
Copia archivos o directorios desde la máquina host al sistema de archivos del contenedor. Se utiliza para agregar archivos, scripts u otros recursos necesarios para la aplicación en la imagen de contenedor. Por ejemplo, COPY ./mi-app /app copiará el directorio mi-app desde la máquina host al directorio /app en el contenedor.

### ADD
Copia archivos o directorios desde el sistema de archivos de la máquina host al sistema de archivos del contenedor. Aunque es similar a la instrucción COPY, ADD tiene algunas características adicionales, como la capacidad de descomprimir automáticamente archivos y de copiar archivos desde una URL remota.

### CMD 
Define el comando predeterminado que se ejecutará cuando se inicie el contenedor. Puede haber solo una instrucción CMD en un Dockerfile. Si se proporciona más de una, solo la última tendrá efecto. Por ejemplo, CMD ["nginx", "-g", "daemon off;"] ejecutará el servidor web Nginx en modo daemon cuando se inicie el contenedor.

### ENTRYPOINT
Configura el comando o el script que se ejecutará cuando se inicie un contenedor basado en la imagen. A diferencia de la instrucción CMD, que especifica el comando predeterminado que se ejecutará y que puede ser sobrescrito desde la línea de comandos al iniciar el contenedor, ENTRYPOINT establece un comando que no se puede sobrescribir fácilmente al iniciar el contenedor.
Es importante destacar que, si se proporciona un comando en la línea de comandos al iniciar el contenedor (por ejemplo, docker run <imagen> <comando>), este comando se agregará como argumentos al comando especificado en ENTRYPOINT, en lugar de sobrescribirlo. Esto permite que el contenedor sea más versátil y se adapte a diferentes necesidades de uso.

### EXPOSE
Informa a Docker que el contenedor escuchará en un puerto específico en tiempo de ejecución. No abre realmente el puerto, solo documenta que la aplicación dentro del contenedor puede usar dicho puerto. Por ejemplo, EXPOSE 80 expone el puerto 80 en el contenedor, lo que permite que se acceda a la aplicación que se esté ejecutando en ese puerto desde fuera del contenedor.

### ENV
Define variables de entorno dentro del contenedor. Las variables de entorno definidas con ENV estarán disponibles para cualquier proceso en el contenedor durante su ejecución. Por ejemplo, ENV MYSQL_ROOT_PASSWORD=password define una variable de entorno llamada MYSQL_ROOT_PASSWORD con el valor password.

### VOLUME 
Esta instrucción se se utiliza para definir un punto de montaje para volúmenes dentro del contenedor.

##  Para construir una imagen de Docker a partir de un Dockerfile
```
docker build -t <nombre imagen>:<tag> .
```
- **-t** esta opción se utiliza para etiquetar la imagen que se está construyendo con un nombre y una versión.
- <nombre_imagen>:<tag> por ejemplo: myapp:1.0
- **.** este punto indica al comando docker build que busque el Dockerfile en el directorio actual, es decir especifica la ubicación del contexto de la construcción que incluye el Dockerfile y cualquier otro archivo necesario para la construcción de la imagen.

## Ejemplo
### Colocar las siguientes instrucciones en un Dockerfile, 
![Dockerfile](img/Dockerfile.PNG)

- apachectl: Es el script de control para el servidor web Apache. Se utiliza para iniciar, detener y controlar el servidor web.
- -D FOREGROUND: Esta opción le dice a Apache que se ejecute en primer plano. Por defecto, Apache se ejecuta como un servicio en segundo plano. Sin embargo, en un contenedor Docker, es preferible que el proceso principal (en este caso, Apache) se ejecute en primer plano para que Docker pueda monitorear el estado del proceso. Si Apache se ejecutara en segundo plano, Docker no podría saber si el servidor web está funcionando correctamente o no.

 
### Ejecutar el archivo Dockerfile y construir una imagen en la versión 1.0
No olvides verificar en qué directorio se encuentra el archivo Dockerfile
```
docker build -t mi-apache:1.0 .
```

**¿Cuántos pasos se han ejecutado?**
# RESPONDER 

Se corrieron solo 4 pasos y luego surgio un error, esto sucede porque la imagen de centos:7 esta descontinuada y ya no funciona con normalidad.


<img width="1449" height="630" alt="image" src="https://github.com/user-attachments/assets/6257cb72-184a-4b64-9dde-735092b89251" />

Por tanto se debe hacer una modificacion al Dockerfile

<img width="433" height="188" alt="image" src="https://github.com/user-attachments/assets/f67cdf70-0796-4c19-b836-1af14af291e0" />


Corregido esto se ejecutan 4 pasos

<img width="1438" height="591" alt="image" src="https://github.com/user-attachments/assets/bfb80523-bc4c-4602-8adb-8a1cbfb4f4af" />

### Inspeccionar la imagen creada
# COMPLETAR CON UNA CAPTURA

<img width="1118" height="647" alt="image" src="https://github.com/user-attachments/assets/9b7f4a7b-95af-4037-855e-7bb9a47cfdc3" />

**Modificar el archivo index.html para incluir su nombre y luego crear una nueva versión de la imagen anterior**
**¿Cuántos pasos se han ejecutado? ¿Observa algo diferente en la creación de la imagen**
<img width="1457" height="550" alt="image" src="https://github.com/user-attachments/assets/5f510828-df4e-4e7e-8975-b6ab83af9915" />

Se ejecutaron nuevamente 4 pasos pero los pasos que ya fueron realizados en la anterior version y no han habido cambios (para mi caso el nombre modificado de index.html ya fue cambiado en la primera version de la imagen) todos aparecen como CACHED, y el tiempo de creacion dura mucho menos.

## Mecanismo de caché
Docker usa un mecanismo de caché cuando crea imágenes para acelerar el proceso de construcción y evitar la repetición de pasos que no han cambiado. Cada instrucción en un Dockerfile crea una capa en la imagen final. Docker intenta reutilizar las capas de una construcción anterior si no han cambiado, lo que reduce significativamente el tiempo de construcción.

- Instrucción FROM: Si la imagen base ya está en el sistema, Docker la reutiliza.
- Instrucciones de configuración (ENV, RUN, COPY, etc.): Docker verifica si alguna instrucción ha cambiado. Si no, reutiliza la capa correspondiente de la caché.
- Instrucción COPY y ADD: Si los archivos copiados no han cambiado, Docker reutiliza la capa de caché correspondiente.
![mapeo](img/dockerfile-cache.PNG)

### Crear un contenedor a partir de las imagen creada, mapear todos los puertos
```
docker run -d --name miapache -P mi-apache:2.0
```

### ¿Con que puerto host se está realizando el mapeo?
# COMPLETAR CON LA RESPUESTA
32768
<img width="1908" height="54" alt="image" src="https://github.com/user-attachments/assets/5b629b17-1488-4c41-8b21-e576a211e81e" />


**¿Qué es una imagen huérfana?**
# COMPLETAR CON LA RESPUESTA
Es una imagen que no tiene etiqueta (tag) y no está asociada a ningún contenedor, por lo que no está siendo utilizada.

### Identificar imágenes huérfanas
```
docker images -f "dangling=true"
```

### Listar los IDS de las imágenes huérfanas
```
docker images -f "dangling=true" -q
```

### Eliminar imágenes huérfanas
Este comando eliminará todas las imágenes que no estén asociadas a ningún contenedor en ejecución. Antes de ejecutarlo, asegúrate de revisar las imágenes que serán eliminadas para evitar la pérdida de imágenes importantes. 
```
docker image prune
```

### Para Ejecutar un archivo Dockerfile que tiene otro nombre
```
docker build -t <nombre imagen>:<tag> -f <ruta y nombre del Dockerfile> .
```

## Por ejemplo
docker build -t imagen:1.0 -f Dockerfile-custom .



