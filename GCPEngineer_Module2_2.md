# GCP Engineer - Core Infraestructure (Module 2 - Part 2)

## Cloud Storage

![[Pasted image 20251030224010.png]]

- Ofrece un almacenamiento de objetos durable y con alta disponibilidad
- El **almacenamiento de objetos** es una arquitectura de almacenamiento de datos que los administra como objetos
- Estos objetos se almacenan en un formato empaquetado que contiene **forma binaria** de los datos reales como los metadatos.
- Entre los datos se incluyen: videos, imagenes y grabaciones de audio
- **Cloud Storage** es el servicio de almacenamiento de objetos
- Suele usarse para almacenar grandes objetos binarios (BLOB)
- Se organizan en buckets. El cual un bucket debe tener un nombre y su ubicacion puede afectar la latencia de la misma.



![[Pasted image 20251030224301.png]]
- Los objetos no se pueden modificar, son inmutables, sino que se crean versiones de la misma
- Las versiones nuevas reemplazaran las viejas
- El control de versiones permite ver versiones archivadas del objeto

### Tipos principales en Cloud Storage
![[Pasted image 20251030230356.png]]
- **Standard Storage:** es ideal para datos de acceso frecuente o "activos". Tambien para datos que se almacenan durante periodos breves.
- **Nearline Storage**: ideal para almacenar datos de acceso poco frecuente, como leer o modificar un datos al mes. Ejemplo: copia de seguridad, contenido multimedia larga o datos archivados.
- **Coldline Storage:** es una opcion de bajo costo para datos de acceso poco frecuente, a diferncia de Nearline, esta diseñada para leer una vez cada 90 dias.
- **Archive Storage:** es la de menor costo y se usa para archivar datos, copias de seguridad en linea y la recuperacion de datos ante desastres. Es ideal para el acceso a los datos 1 vez al año.


### AutoClass
![[Pasted image 20251030231145.png]]
- Cloud Storage tambien ofrece una nueva herramienta denominada **AutoClass**, el cual es una funcion que traslada automaticamente los objetos a la clase de almacenamiento ideal segun sus patrones de acceso.
- Los datos menos accedidos los traslada automaticamente en el **coldline storage**
- Los datos mas accedidos los traslada automaticamente en la **standard storage**
- Automatiz el ahorro de costos
- No tiene tarifa minima porque pagas por su uso
- El servidor encripta los datos antes de escribirlos en el disco.
- Los **datos del lado del cliente** se encriptan en HTTPS/TLS (capa de transporte
- Existen varias maneras de transferir la informacion:
	- podemos usar gcloud o el mismo Google Console para transferir la informacion.
	- Si queremos importar terabytes de informacion tenemos **"Storage Transfer Service"** que permite hacerlo de forma rapida y economica. Este servicio permite programar y administrar transferencias por lotes para Cloud Storage
	- Tambien tenemos **Transfer Appliance**, un servidor de almacenamiento de bastidores y de alta capacidad que puedes arrendar de Google Cloud. Podemos transferir hasta un *Petabyte*

## Cloud SQL
![[Pasted image 20251030232407.png]]
- Ofrece bases de datos relacionales administradas con MYSQL, PostgreSQL y SQL Server como servicio.
- Puede escalar hasta 128 nucleos de procesamiento, 864 GB de RAM y 64TB de almacenamiento.
- Usa **replicacion automatica**
- Admite copias de seguridad
- Incluye firewall de red, que controla el acceso de red a instancias de la bases de datos


## Spanner
![[Pasted image 20251030232456.png]]
- Spanner es un servicio administrado de bases de datos relacionales que escala de forma de manera horizontal.
- Funciona con SQL
- Spanner es el servicio que impulsa el negocio de la empresa de Google por $80000 millones
- Es adecuado para alta disponibilidad, coherencia global y una gran cantidad de operaciones por segundos.

## FireStore
![[Pasted image 20251030232810.png]]
- FireStore es una base de datos en la nube NoSQL para desarrollar  soluciones moviles, web y de servidores.
- Aca los datos se organizan en formato de documentos y se organizan en colecciones
- Los documentos pueden contener objetos anidados complejos, ademas de subcolecciones.
- Incluyen filtros y ordenamiento
- Almacena ademas en cache de los datos que usa la App de modo que pueda **escribir, leer,  escuchar y consultar los datos**

## BigTable
![[Pasted image 20251030233204.png]]

- **BigTable** es el servicio de bases de datos NoSQL de Google.
- puede administrar grandes cargas de trabajo con baja latencia y alta capacidad de procesamiento
- Es ideal para apps de operativas y analiticas. (analisis de usuarios, analisis financieros, IoT)
- Los datos pueden trasmitirse con frameworks de **streaming** populares como  Dataflow, Spark Streaming y Storm
- Tambien por **procesamiento por lotes** como: Hadoop, Dataflow o Spark.