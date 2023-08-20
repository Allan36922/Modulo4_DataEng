
#  Datapath 
#  Programa: Data Engineering Program 13va edición

#  Módulo: Data Engineering with Python

Entrega del proyecto Puesta en producción de pipelines & Proyecto utilizando Microsoft Azure y Azure Data Factory.

# Documentación:

## Configuración previa:
- Antes de comenzar, asegúrate de tener una cuenta de Azure y acceso a Azure Data Factory. 
- También necesitarás tener el archivo CSV "Datosaln.csv" almacenado en tu Azure Data Store.

## 1 Paso: Configuración Grupo de recursos
- Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. 
- El grupo de recursos puede incluir todos los recursos de la solución o solo aquellos que se desean administrar como grupo. 
- Por lo general, se recomienda agregar recursos que compartan el mismo ciclo de vida al mismo grupo de recursos para que los pueda implementar, actualizar y eliminar con facilidad como un grupo.
- Los grupos de recursos almacenan metadatos acerca de los recursos.

1. Inicia sesión en tu portal de Azure.
2. Busca "Grupos de recursos" en el panel de búsqueda y "Crear un grupo de recursos" si aún no tienes uno.
3. En el tab de Datos Básico, busca detalles del proyecto, selecciona la suscripción Azure correspondiente y debes indicar el nombre del "Grupo de Recursos".
4. En el tab de Datos Básico, busca detalles del proyecto, en la sección de Detalles de recurso debes seleccionar la región a utilizar.
5. En el tab de Etiquetas, ingresa los datos de clave valor que pueden ser (Creado: User) 
6. En el tab de Datos básico, busca detalles del proyecto, 
7. En el tab de Revisar y crear, si todos los valores están correctos te dejara crear el Grupo de recursos. 

### 2 Paso: Configurar el Azure Data Store**

- Una cuenta de Azure Storage contiene todos los objetos de datos de Azure Storage: blobs, archivos, colas y tablas. 
- La cuenta de almacenamiento proporciona un espacio de nombres único para los datos de Azure Storage que es accesible 
desde cualquier lugar del mundo mediante HTTP o HTTPS. Los datos de la cuenta de almacenamiento son duraderos y 
altamente disponibles, seguros y escalables a gran escala. 

1. Inicia sesión en tu portal de Azure.
2. Busca "cuenta de almacenamiento" en el panel de búsqueda y ingresa a "Crear una cuenta de almacenamiento" si aún no tienes uno.
3. En el tab de Datos básico, busca detalles de la instancia, y agrega el "Nombre de la cuenta de almacenamiento".
4. En el tab de Datos básico, busca detalles de la instancia, la sección región se debe elegir la correspondiente al proyecto, 
   luego en la sección de rendimiento se deja como esta por defecto y para finalizar la Redundancia se debe poner en "Almacenamiento con redundancia loca (LRS)".
5. En siguientes tabs de "Opciones avanzadas", "Redes", "protección de datos”, “Cifrado" y "Etiquetas" se dejan por defecto como están para este proyecto de práctica.
6. En el tab de Revisar y crear, si todos los valores están correctos te dejara crear la cuenta de almacenamiento. 

- Para nuestro ejercicio practico se crearon 2 cuentas de almacenamiento, la primera emulado el ambiente On-premise y otro emulando el ambiente Datalake

ambiente On-premise 
Storage: storagecovidbs
Contenedor: populationbs

ambiente Datalake 
Storage: storagecoviddlake
Contenedor: raw

### 3 Paso: Crear Factorías de datos**

- Azure Data Factory es un servicio gestionado en la nube ideal para proyectos complejos híbridos de extracción-transformación-carga (ETL), extracción-carga-transformación (ELT) e integración de datos.

1. Inicia sesión en tu portal de Azure.
2. Busca "Azure Data Factory" en el panel de búsqueda y crea un nuevo Data Factory si aún no tienes uno.
3. En el tab de Datos básico, la sección detalles del proyecto, se debe seleccionar la suscripción y el grupo de recursos.
4. En el tab de Datos básico, busca detalles del proyecto, En la sección de Detalles de recurso debes seleccionar la región a utilizar además la sección de la versión se deja con el valor predeterminado.
5. En siguientes tabs de "Configuración de Git ","Redes", "Avanzado" y "Etiquetas" se dejan por defecto como están para este proyecto de práctica.
6. En el tab de Revisar y crear, si todos los valores están correctos te dejara crear la Factorías de datos. 

- Para nuestro ejercicio práctico se creó la Factoría llamada dfactcovid19result

### 4 Paso: Crear un Linked Service**

- Un Linked Service es una conexión a una fuente de datos en Azure Data Factory. En este caso, necesitarás crear un Linked Service para tu Azure Data Store:

1. Inicia sesión en tu portal de Azure.
2. Busca "Azure Data Factory" en el panel de búsqueda y crea un nuevo Data Factory si aún no tienes uno.
3. En el menú izquierdo, selecciona "Manage" e ingresar a Linked Service.
4. Buscar "Nuevo" y te llevara a escoger el tipo de Linked Service adecuado para tu Azure Data Store (por ejemplo, Azure Blob Storage, Azure Data Lake Storage Gen1/Gen2, etc.)
   debes cambiar el nombre del Linked Service con las reglas de nomenclatura de la empresa,
5. Completa los detalles solicitados, como la información de autenticación y la ubicación del almacenamiento.

### 5 Paso: Crear un Dataset**

- Un Dataset define la ubicación y el formato de los datos que deseas usar en tus flujos de trabajo. En este caso, crearás un Dataset para el archivo CSV:

1. En el Editor de Azure Data Factory, selecciona "Author & Monitor" nuevamente.
2. En la barra de herramientas superior, elige "Author" (Autor) y luego selecciona "New Dataset" (Nuevo Conjunto de Datos).
3. Selecciona el tipo de Dataset adecuado según el Linked Service que hayas creado anteriormente (por ejemplo, Azure Blob Dataset o Azure Data Lake Dataset).
4. Completa los detalles, como la ruta del archivo "Datosaln.csv" en tu Azure Data Store.

### 6 Paso: Crear un Copy Data Activity**

- Un Copy Data Activity es la acción que moverá los datos desde la fuente (tu Dataset) hasta el destino que elijas. En este caso, copiarás los datos del archivo CSV a un destino de tu elección:

1. En el Editor de Azure Data Factory, selecciona "Author & Monitor".
2. En la barra de herramientas superior, elige "Author" y luego selecciona "New Pipeline" (Nuevo Canal).
3. En la ventana de diseño del canal, arrastra y suelta "Copy Data" (Copiar Datos) desde la barra de herramientas izquierda.
4. Configura el origen y el destino del Copy Data Activity:
   - Origen: Selecciona el Dataset que creaste para el archivo CSV.
   - Destino: Puedes elegir otro Dataset o incluso una base de datos u otro destino compatible.
5. Configura las opciones de mapeo de columnas si es necesario para que coincidan las columnas de origen y destino.
6. Guarda y publica el canal.

### 7 Paso: Ejecutar el Pipeline**

- Una vez que hayas configurado el canal, es hora de ejecutarlo:

1. En el Editor de Azure Data Factory, selecciona "Author & Monitor".
2. Encuentra el Pipeline que creaste y haz clic en "Debug" (Depurar) para iniciar la ejecución.
3. Monitorea el progreso y verifica si hay errores en la ventana de "Monitor".

- Estatus completado la ingesta de datos desde el archivo CSV en tu Azure Data Store utilizando Azure Data Factory.
