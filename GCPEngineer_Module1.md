# GCP Engineer Module 1 - Preparacion para el proceso de certificacion Associate Cloud Engineer

## Introduccion
Un Associate Cloud Engineer implementa y protege aplicaciones e infraestructura, supervisa  operaciones de varios proyectos y mantiene soluciones empresariales.
Este rol, tiene experiencia en nubes publicas y soluciones locales, sabe como usar Google Cloud Console y la interfaz de la linea de comandos.

La ruta de aprendizaje para dar el certificado, es esta recomendacion: 
![[Pasted image 20251018124030.png]]



## Problematica 
Vamos a empezar a resolver una problematica el cual gira en torno a la implementación de una arquitectura de nube para gestionar recursos de procesamiento, almacenamiento y red, como parte de un desafío de aprendizaje para un ingeniero de nube asociado. Es para una aplicacion local que se quiere migrar a la nube de Google Cloud.

### Configuracion del entorno de la nube de Cymbal Superstore
Para la configuracion del entorno de la misma:
- El equipo empezara  con una reunion de planificacion para decidir como se organizara la estructura de Cymbal Superstore
- Los resultados de esta fase de migracion incluyen: **crear una jerarquia de recursos, implementar politicas de la organizacion, administrar proyectos y cuotas, administrar usuarios y grupos , y aplicar la administracion de accesos**
	- Tambien incluye de configurar la facturacion y la supervision de los recursos de la nube

#### Jerarquia de recursos de Cymbal Superstore
La configuracion de una jerarquia de recursos depende de las necesidades  y la estructura de la organizacion, 
**Cymba Superstore** planea migrar 3 aplicaciones:
- Aplicacion de cadena de suministros, que se va ejecutar en VMS
- Aplicacion de comercio electronico, alojada en contenedores que usara GKE (Google Kubernetes)
- Aplicacion de transporte, que usara Cloud Run Functions para seguir la ubicacion de los camiones.

![[Pasted image 20251018131025.png]]
- Se puede oberservar que cada aplicacion es administrada por un departamento diferente (Operaciones, Sales & Marketing, Logistics)
- Existen varios proyectos para la aplicacion de cadena de suministros. 
	- Una para cada entorno : **entorno de desarrollo, una de pruebas y otra para produccion**
- Despues de definir la jerquia, **se debe establecer las politicas de la organizacion**
	- Marketing necesita acceder (permiso de lectura) a los datos de suministro, para ver si los niveles de inventario afectaron a las campañas de marketing
- Ademas de habilitar las APIs necesarias, ya que se va optar por un GKE, Cloud SQL como backend y otras.


![[Pasted image 20251018133632.png]]
- Tambien incluye otorgar roles  de IAM a los miembros para garantizar que tengan el acceso correcto a los proyectos.
	- Se aconseja usar grupos de usuarios y NO hacer por usuarios la asignacion de permisos

#### Google Cloud Observability
Este servicio proporciona metricas y registros para todos los servicios, recursos y proyectos de tu arquitectura de la nube. Para supervisar metricas de varios proyectos, debemos configurar los permisos de proyeccto
![[Pasted image 20251018134405.png]]


#### Cuentas de Facturacion de Cymbal Superstore
Durante la migracion a Google Cloud, Cymbal transferira parte de sus gastos de TI a los gastos operativos, y los departamentos asociados con cada app seran responsables de los costos de procesamiento y almacenamiento

![[Pasted image 20251018134611.png]]
- Deberas crear una cuenta de facturacion por cada grupo y vincular cada proyecto con la cuenta que corresponda

## Evaluacion de la aplicacion para planificar y configurar una solucion en la nube para ellas.
- La reunion de planificacion para migrar a Cymbal Superstore a Google Cloud fue exitosa
- Luego de esto nuestra tarea es el diseño de la arquitectura de la nube durante la fase de migracion: La planificacion y la configuracion de la solucion
- Tenemos opciones de procesamiento las cuales son basadas en VMs o contenedores.
- Tambien otras opciones basadas en recursos y sin servidores disponibles
- El nivel de control y flexibilidad que necesita tu solucion pueden ser factores  para decidir  el producto de procesamiento **adecuado**:
- La capacidad  de procesamiento y la latencia determinan la rapidez de las respuestas.
- La conectividad  de red que necesita tu aplicacion es otro factor a tener en cuenta 


Veamos esto en el ejemplo de Cymbal Superstore:
![[Pasted image 20251025143820.png]]
- Cymbal tiene 3 aplicaciones a migrar a Google Cloud
	- El departamento de Ventas y Marketing de Cymbal tiene un web app con una UI para que los clientes puedan mirar los productos y realicen los pedidos.
- Esta app se basa en contenedores  y debe estar disponible en todo el mundo y tener una latencia baja