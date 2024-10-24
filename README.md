# Spring-Batch
Aprendiendo Spring Batch in Action

**Introducción a Spring Batch**

### Este capítulo cubre:
- Comprender las aplicaciones por Batch en las arquitecturas actuales
- Describir las características principales de Spring Batch
- Leer y escribir datos de manera eficiente
- Implementar el procesamiento dentro de un trabajo con Spring Batch
- Probar un trabajo de Spring Batch

### Aplicaciones Batch

Las aplicaciones Batch son un reto a la hora de desarrollarlas, y por eso se creó ***Spring Batch***: para hacer que sean más fáciles de escribir, pero también más `rápidas`, `robustas` y `confiables`. 

### ¿Qué son las aplicaciones Batch?

Las aplicaciones Batch procesan grandes cantidades de datos sin intervención humana. Se utilizan en escenarios como:
- Generación de estados financieros mensuales
- Cálculo de estadísticas
- Indexación de archivos

### ¿Por qué son un desafío?

Los requisitos de las aplicaciones Batch, como el `manejo de grandes volúmenes de datos`, el `rendimiento` y la `robustez`, las convierten en un reto para implementarlas de manera correcta y eficiente.

### Características de Spring Batch

Una vez que comprendas el panorama general de las aplicaciones Batch, estarás listo para conocer `Spring Batch` y sus características principales:
- Ayuda a procesar datos de manera eficiente con diversas tecnologías, como bases de datos, archivos y colas.
  
Implementaremos un trabajo de Spring Batch basado en un caso real. Al final de este capítulo, tendrás una visión general de lo que hace Spring Batch y estarás listo para implementar tu primer trabajo.

¡Empecemos con las aplicaciones Batch!

### ¿Qué son las aplicaciones Batch?

El escenario más común para una aplicación Batch es la **exportación de datos a archivos desde un sistema y su posterior procesamiento en otro**. 

Por ejemplo, si deseas intercambiar datos entre dos sistemas, puedes exportar datos como archivos desde el sistema A e importarlos en una base de datos en el sistema B. 

**La Figura 1.1 ilustra este ejemplo.**
![Una aplicación Batch típica: el sistema A exporta datos a archivos planos, y el sistema B utiliza un proceso Batch para leer los archivos en una base de datos](image.png)

Las aplicaciones Batch **procesan datos de forma automática**, por lo que deben ser **robustas y confiables**, ya que no hay intervención humana para corregir errores. 

A medida que `aumenta el volumen de datos que debe procesar` una aplicación Batch, el `tiempo necesario para completar el procesamiento también aumenta`. 

Esto significa que también **debes considerar el rendimiento** de la aplicación Batch, ya que a menudo tiene que **ejecutarse dentro de un período de tiempo específico**.

### Requisitos de una aplicación Batch

1. **Gran volumen de datos**: Las aplicaciones Batch deben ser capaces de manejar grandes volúmenes de datos para importar, exportar o realizar cálculos.
2. **Automatización**: Deben ejecutarse sin intervención del usuario, excepto para resolver problemas graves.
3. **Robustez**: Deben ser capaces de manejar datos inválidos sin bloquearse o finalizar de manera prematura.
4. **Confiabilidad**: Deben llevar un registro de lo que sale mal y cuándo (registro de eventos, notificaciones).
5. **Rendimiento**: Deben funcionar bien para completar el procesamiento en una ventana de tiempo específica o para evitar interferir con otras aplicaciones que se estén ejecutando al mismo tiempo.

### Conociendo Spring Batch

El objetivo del proyecto Spring Batch es proporcionar un marco de trabajo orientado a procesos por lotes, de código abierto, que aborde de manera efectiva las necesidades más comunes de las aplicaciones por lotes. 

El proyecto Spring Batch nació en 2007 gracias a la colaboración entre Accenture y SpringSource:
- **Accenture** aportó su experiencia en marcos de trabajo propietarios para procesamiento por lotes.
- **SpringSource** contribuyó con su conocimiento técnico y el modelo de programación de Spring.

### Principales características de Spring Batch (Tabla 1.1)

|     **Característica**   |**Descripción** |
|    -------------------   |----------------|
|**Spring Framework foundations**| Aprovecha el soporte empresarial, la inyección de dependencias y la programación orientada a aspectos.|
|**Batch-oriented processing**| Fomenta las mejores prácticas al leer y escribir datos.                                      |
|**Ready-to-use componentsr**| Proporciona componentes para abordar escenarios comunes (leer y escribir datos en bases de datos y archivos).|
|**Robustness and reliability**| Permite la omisión y el reintento de manera declarativa; habilita el reinicio después de un fallo.    |

### Beneficios de Spring Batch

Al utilizar Spring Batch, obtienes los beneficios de las mejores prácticas que el marco de trabajo implementa y fomenta, así como una variedad de componentes disponibles para los formatos y tecnologías más populares en la industria del software.

### Tecnologías de almacenamiento compatibles con Spring Batch (Tabla 1.2)

|**Tipo de fuente de datos**|**Tecnología**|**Descripción**|
|---------------------------|--------------|---------------|
| **Database**           | JDBC                       | Utiliza paginación, cursores y actualizaciones por lotes.|
| **Database**           | Hibernate                  | Utiliza paginación y cursores.                   |
| **Database**           | JPA (Java Persistence API) | Utiliza paginación.                              |

Tecnologías de lectura y escritura compatibles con Spring Batch (continuación)
|Tipo de fuente de datos |Technology |Description|
|---------------------------|--------------|---------------|
|Database|iBATIS|Utiliza paginación para manejar los datos.|
|File|Flat file|Soporta archivos planos con delimitadores y de longitud fija|
|File|xml|Utiliza StAX (API de Streaming para XML) para el análisis; se basa en Spring OXM y soporta JAXB (Java Architecture for XML Binding), XStream y Castor.|



Spring Batch es bastante versátil gracias a su soporte nativo para diversas tecnologías. Este soporte se estudia en detalle en los capítulos 5 y 6.

### ¿Cómo cumple Spring Batch con los requisitos de robustez y confiabilidad?

Spring Batch permite implementar aplicaciones robustas y confiables mediante:
- **Reintentos y omisión declarativa de errores**, lo que facilita manejar excepciones sin abortar el procesamiento.
- **Capacidades de reinicio**, que permiten reanudar el trabajo desde donde ocurrió un fallo.

---
Aquí tienes la traducción y el resumen sobre la robustez, la confiabilidad y las estrategias de escalabilidad en Spring Batch:

---

### Robustez y confiabilidad

¿Debería fallar un trabajo por lotes completo por una línea mal formateada? No siempre. En Spring Batch, la decisión de omitir una línea o un elemento incorrecto es declarativa y se realiza mediante la configuración.

### ¿Qué sucede si reinicias un trabajo por lotes que falló?

Si un trabajo falla, ¿debería reiniciarse desde el principio, lo que podría causar la corrupción de datos al procesar elementos nuevamente, o debería ser capaz de reiniciar exactamente donde se quedó? Spring Batch facilita esto último: los componentes pueden rastrear todas sus acciones, y el marco les proporciona los datos de ejecución al reiniciar, lo que les permite continuar el procesamiento en el lugar correcto.

### Cumplimiento de los requisitos de robustez y confiabilidad

Spring Batch aborda estos requisitos proporcionando capacidades de reintento, omisión y reinicio. El Capítulo 2 ofrece una visión general de los reinicios, mientras que el Capítulo 8 cubre la robustez en detalle.

### Requisitos de rendimiento

El rendimiento es otro aspecto crucial para las aplicaciones por lotes. Para cumplir con este requisito, Spring Batch permite el procesamiento en `chunks`(porciones).

### Estrategias de escalabilidad

Spring Batch procesa elementos en pequeños `chunks`. La idea es que un trabajo lea y escriba elementos en bloques pequeños, lo que permite el flujo continuo de datos en lugar de cargar todos los datos en memoria.

Por defecto, el procesamiento en porciones es de un solo subproceso (`single-threaded`) y suele ser eficiente, pero algunos trabajos requieren una ejecución más rápida. Para ello, Spring Batch proporciona métodos para:

- **Procesamiento multihilo (multithreaded)**: Permite ejecutar el procesamiento en múltiples hilos para mejorar la velocidad.
- **Distribución del procesamiento en múltiples nodos físicos**: Reparte la carga de trabajo entre diferentes servidores.

### Particionamiento

El particionamiento divide un paso (step) en subpasos, cada uno de los cuales maneja una parte específica de los datos. Esto implica conocer la estructura de los datos de entrada y saber cómo distribuirlos entre los subpasos. La distribución puede realizarse por rangos de valores de clave primaria en registros de bases de datos o por directorios para archivos. Los subpasos pueden ejecutarse de manera local o remota, y Spring Batch ofrece soporte para subpasos multihilo.

La Figura 1.2 ilustra un ejemplo de particionamiento basado en nombres de archivos: A-D, E-H, y así sucesivamente hasta Y-Z.

---

Con esto, hemos finalizado el recorrido por las características más importantes de Spring Batch y los beneficios que aporta a tus aplicaciones. Ahora es el momento de pasar a la parte práctica con la implementación de un trabajo por lotes en un escenario real.

¿Te gustaría empezar con un ejemplo práctico para ver cómo se implementa un trabajo en Spring Batch?