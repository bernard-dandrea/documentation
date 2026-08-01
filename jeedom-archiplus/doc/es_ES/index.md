<!--  
Última modificación: 26/06/2026
-->
- [La gestión de los historiales en Jeedom](#la-gestion-de-los-historiales-en-jeedom)
  - [Funcionamiento](#funcionamiento)
  - [Volumen de los historiales](#volumen-de-los-historiales)
  - [Las limitaciones del archivado en Jeedom](#las-limitaciones-del-archivado-en-jeedom)
  - [Las ventajas del plugin Archiplus](#las-ventajas-del-plugin-archiplus)
  - [Aviso](#aviso)
- [Complemento Archiplus](#plugin-archiplus)
  - [Instalar el complemento archiplus](#instalar-el-complemento-archiplus)
  - [Configurar el complemento](#configurar-el-complemento)
  - [Los módulos del complemento](#los-módulos-del-complemento)
- [Acceso a los módulos](#acceso-a-los-módulos)
  - [Los botones de control](#los-botones-de-control)
  - [La columna de selección de líneas](#la-columna-de-selección-de-líneas)
  - [Los encabezados de columna](#los-encabezados-de-columna)
  - [Las líneas](#las-líneas)
  - [Los totales al pie de la tabla](#los-totales-al-pie-de-la-tabla)
- [el módulo Monitor](#el-módulo-monitor)
  - [Estadísticas](#estadísticas)
  - [Visualización](#visualización)
  - [Modificaciones](#modificaciones)
  - [Modificaciones a partir de un archivo Excel](#modificaciones-a-partir-de-un-archivo-excel)
  - [Datos modificables](#datos-modificables)
    - [KLV (Keep Last Value)](#klv-keep-last-value)
    - [Uniq](#uniq)
    - [Plazo](#plazo)
    - [Encuadre](#encuadre)
    - [Pond](#pond)
    - [Paquete](#pack)
    - [Redondeado](#redondeado)
  - [Funciones accesibles a través del menú contextual](#funciones-accesibles-a-través-del-menú-contextual)
- [Datos históricos](#datos-históricos)
  - [Acceso](#acceso)
  - [Modificación](#modificación)
  - [Eliminación](#eliminación)
  - [Exportar](#export)
- [El módulo Import](#el-módulo-import)
- [El módulo Restore](#el-módulo-restore)
- [Preguntas frecuentes](#faq)
  - [Mantener el último valor](#keep-last-value)
  - [Uniq](#uniq-1)
  - [Plazos y alcance](#plazos-y-alcance)
  - [Suavizado y ponderación](#suavizado-y-ponderación)
  - [Paquete](#pack-1)
  - [Redondeado](#redondeado-1)
  - [Copiar los datos de historyArch a history](#copiar-los-datos-de-historyarch-a-history)
  - [Cómo utilizar Archiplus en PHP](#cómo-utilizar-archiplus-en-php)
- [Los registros](#los-registros)
- [Traducción](#traducción)
- [Opiniones](#opiniones)



La función principal del complemento es proporcionar un conjunto completo de herramientas que permiten:

*   **gestionar los parámetros de archivo de los pedidos de tipo INFO**
*   **visualizar los volúmenes de datos y detectar anomalías**
*   **insertar fácilmente datos históricos a partir de archivos tipo Excel**
*   **cómo recuperar los historiales de los archivos de Jeedom**
*   **ampliar las opciones de archivo estándar de Jeedom**

La activación opcional del archivado integrado en el complemento permite ampliar de forma muy significativa las funciones de archivado que ofrece Jeedom.

# Gestión de historiales en Jeedom

## Funcionamiento

El historial de Jeedom ha cambiado poco desde las primeras versiones y se basa en dos tablas:

* la tabla «history», que recibe las actualizaciones de los valores de los comandos de tipo INFO para los que se ha activado el historial
* la tabla historyArch, que recibe en cada proceso de archivo (normalmente cada día a las 5:00) los valores históricos, consolidados o no, según la configuración definida para el comando.

La estructura de ambas tablas es idéntica y muy sencilla: se registra un valor por cada pedido con el ID y la fecha y hora (con precisión de segundos).

El historial se puede visualizar en la interfaz de Jeedom en forma de gráfico.

La documentación oficial sobre la gestión de historiales en Jeedom se encuentra [aquí](https://doc.jeedom.com/fr_FR/core/4.5/history).

## Volumen de los historiales

El usuario de Jeedom empezará a interesarse por el historial cuando observe que la base de datos crece de forma desmesurada, que los tiempos de visualización del historial se alargan mucho y que el tamaño de las copias de seguridad no deja de aumentar.

El siguiente enlace te permite acceder a un tutorial que explica cómo crear un escenario que muestre los volúmenes de las tablas más grandes y las órdenes INFO con los historiales más extensos [Tutorial: Analizar los archivos](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

En pocas palabras, puedes consultar el tamaño de las tablas consultando directamente la base de datos (menú Ajustes / Sistema / Configuración; a continuación, la pestaña SO / BD (la última); después, el botón «Administración de la base de datos» (el botón rojo más abajo); y, a la izquierda, la consulta «tamaño»).

En una instalación estándar, hay que empezar a plantearse la cuestión cuando el volumen total de los archivos supere el millón de registros o cuando una consulta «info» arroje más de 10 000 registros. En ese caso, es necesario analizar las órdenes en cuestión y ajustar los distintos parámetros de historización y archivo para reducir dicho volumen. Si esto no es posible, quizá haya que recurrir a otros métodos de archivo, como, por ejemplo, InfluxDB, que puede integrarse de forma estándar con Jeedom.

El complemento archiplus muestra al instante los volúmenes de history y historyArch, lo que permite identificar fácilmente los problemas y aportar soluciones.

## Las limitaciones del almacenamiento en Jeedom

Aunque en muchas instalaciones el funcionamiento estándar será suficiente, cabe señalar las siguientes limitaciones:

* Dificultad para visualizar y modificar los parámetros de archivo: la única herramienta disponible (menú «Análisis» / «Historial» y, a continuación, «Configuración») es muy lenta, poco práctica y ofrece pocos campos que configurar
* Dificultad para visualizar los volúmenes históricos por pedido e identificar los volúmenes anómalos: hay que recurrir a consultas SQL y procesos poco prácticos
* Parámetros para la agrupación de datos en historyArch definidos de forma global y no personalizables por comando
* Falta de visibilidad sobre el proceso de archivado (no hay registro)
* Archivado global: no es posible iniciar el archivado para un pedido concreto
* Suavizado por media aproximada
* Herramientas básicas para exportar e importar datos (complemento dataexport). No se ofrece ninguna opción para restaurar los datos históricos contenidos en las copias de seguridad.

## Las ventajas del plugin archiplus

El complemento archiplus permite visualizar en una tabla los comandos de tipo INFO junto con todos los parámetros relacionados con el archivado. También se muestra el número de registros en history y historyArch, lo que permite detectar con gran facilidad los volúmenes excesivos. El complemento utiliza la biblioteca JavaScript Tabulator, que es extremadamente eficaz y permite un acceso muy sencillo a las funciones del complemento.

Todas las funciones que ofrece Jeedom están disponibles directamente y se han añadido otras nuevas:

* Configuración avanzada del control
* Visualización de gráficos y extracción de datos
* Borrar el historial
* Exportación estándar en formato CSV
* Copiar la configuración del historial (o un único parámetro) a varios comandos
* Carga de los parámetros de los comandos INFO relativos al historial desde un archivo Excel
* Inicio del archivo de un pedido concreto
* Copiar el historial de un pedido a otro pedido
* Copia de historyArch a history para iniciar una consolidación por intervalos
* Importación del historial de un pedido desde un archivo de Excel
* Extracción del historial en varios formatos (xlsx, CSV, JSON, HTML) de uno o varios comandos desde Jeedom o desde una copia de seguridad estándar de Jeedom
* Extracción de una copia de seguridad de Jeedom de los parámetros de los comandos INFO relativos al historial (estos parámetros se pueden aplicar posteriormente en Jeedom)

Además, el proceso de archivado del complemento se puede activar en sustitución de la función de archivado nativa que ofrece Jeedom. Esto permite:

* iniciar el archivo de un pedido concreto
* registrar en el registro Archiplus todas las operaciones realizadas y los parámetros tenidos en cuenta para cada comando
* personalizar el periodo de cálculo (mínimo, máximo, media), el plazo antes del archivo y el tamaño del paquete para cada comando
* ajustar la fecha de purga a un día, una hora o un minuto
* Iniciar el archivado de un pedido desde un escenario (en código PHP)
* añadir opciones no previstas en Jeedom (véanse las explicaciones más adelante en la documentación)
  * Keep Last Value: conservar siempre al menos un valor en el historial
  * Uniq: eliminar los valores consecutivos idénticos en historyArch
  * Ponderación: en el suavizado por media, calcular el valor ponderado a lo largo del intervalo (y no la media de los valores)

El complemento archiplus se ha desarrollado en Debian 12 y no utiliza jQuery (al igual que las bibliotecas de terceros empleadas). Cumple con los estándares de desarrollo de Jeedom. El código de la clase archiplus está muy bien estructurado y ampliamente documentado: el autor del complemento estudiará todas las propuestas de corrección o mejora.

Dado que Jeedom no tiene previsto desarrollar la gestión del historial, no debería ser necesario rediseñar el complemento en un futuro próximo.

## Advertencia

El complemento y su proceso específico de archivado se han sometido a pruebas muy rigurosas, pero no por ello están exentos de posibles anomalías. En tal caso, el equipo de Jeedom no está, evidentemente, obligado a prestar asistencia. Las solicitudes de análisis y corrección deberán dirigirse obligatoriamente al autor del complemento a través del formulario estándar de asistencia técnica.

La activación del complemento y, en particular, del proceso de archivado, implica, por lo tanto, la plena aceptación de esta situación.

# Complemento Archiplus

## Instalar el complemento archiplus

Ve al Market de Jeedom, busca el complemento archiplus e instala la versión **estable**. A continuación, **activa el complemento**.

![001](../images/001.png)

Se puede acceder al complemento a través del menú.

## Configurar el complemento

En la configuración, puedes ajustar los parámetros habituales de los complementos y los valores por defecto del complemento.

![003](../images/003.png)

Para obtener la máxima información sobre el proceso de archivado del complemento y las acciones realizadas, se recomienda poner los registros en modo «Debug».

Ten en cuenta que las solicitudes de asistencia deberán realizarse a través del botón **Asistencia**.

![002](../images/002.png)

En la sección de configuración, puedes:

* Activar el archivado específico (desactivado por defecto)
* Indica si deben eliminarse las entradas de history y historyArch en caso de que el comando en cuestión no exista
* Optar por no transferir los registros de history a historyArch cuando no haya suavizado
* Definir el formato para las exportaciones
* Definir el marco predeterminado para las fechas de eliminación y de finalización del archivo

Al activar el archivado específico, se crea una nueva tarea programada en el motor de tareas y se desactiva el archivado estándar. Al desactivar el archivado específico, se realiza la operación inversa.

Si quieres probar el proceso de archivado del complemento, puedes activarlo temporalmente, realizar pruebas de archivado en comandos individuales y, a continuación, desactivar el archivado del complemento. Dado que el proceso de archivado de Jeedom suele iniciarse a las 5 de la mañana, no habrá ningún impacto en los comandos no probados.

## Los módulos del complemento

![004](../images/004.png)

Desde el menú Plugins / Monitoring / archiplus, puedes acceder a todas las funciones del plugin

* Configuración del complemento (véase más arriba)
* Acceso a los parámetros generales de la configuración del archivado
* Supervisión: visualizar y modificar la configuración y realizar las principales operaciones relacionadas con el archivo
* Importación: importar datos históricos desde un archivo de tipo Excel
* Restore: extraer los datos históricos de un archivo estándar de Jeedom

Se puede acceder a la visualización de los datos históricos desde el módulo «Monitoring and Restore».

# Acceso a los módulos

Los módulos se inician desde la configuración del complemento.

![005](../images/005.png)

La base de la interfaz es una tabla Tabulator que contiene los datos pertinentes.

Por ejemplo, con el módulo Monitor, se muestra una tabla con los comandos de tipo INFO que tienen activada la función de historial.

La pantalla consta de varias partes.

## Los botones de control

![006](../images/006.png)

Los botones permiten realizar acciones generales relacionadas con la visualización, las líneas seleccionadas, las actualizaciones, etc.

![013](../images/013.png)

Los botones de arriba son comunes a todos los módulos y permiten:

* mostrar el archivo de registro de Archiplus
* ir a la primera o a la última línea de la tabla
* desactivar los filtros que se han activado
* volver a la clasificación inicial
* exportar los datos que aparecen en la tabla (solo los datos filtrados)
* Volver a los distintos módulos que ofrece Archiplus

![019](../images/019.png)

El botón estándar «Ayuda en la página actual» permite acceder a la documentación del complemento.

## La columna de selección de filas

![007](../images/007.png)

La primera columna permite seleccionar las líneas sobre las que se desea actuar.

Al hacer clic en los encabezados de las columnas, se seleccionan todas las filas que aparecen en la tabla.

Se puede seleccionar cada línea por separado haciendo clic en la casilla de selección o en cualquier punto de la línea.

También se puede seleccionar una serie de líneas haciendo clic en la primera de ellas mientras se mantiene pulsada la tecla Control y, a continuación, haciendo clic en la última, siempre manteniendo pulsada la tecla Control (tenga cuidado de hacer clic en cualquier punto de la línea, pero no en la casilla de selección; de lo contrario, la selección múltiple no funcionará).

## Los encabezados de columna

![008](../images/008.png)

Los encabezados de columna describen el contenido de las celdas situadas en la columna.

Permiten:

* obtener información adicional mediante una ventana emergente al mantener el cursor sobre el campo durante un segundo
* ordenar las filas según el valor del campo haciendo clic en el encabezado de la columna (ten en cuenta que el botón «Ordenación inicial» permite anular todas las ordenaciones realizadas)
* filtrar las filas que se muestran introduciendo un criterio de selección en el campo situado debajo del nombre de la columna (ten en cuenta que el botón «Restablecer» permite anular todas las selecciones).

En el caso del módulo Monitor, la agrupación de columnas permite seleccionar únicamente determinados tipos de información.

## Las líneas

![009](../images/009.png)

Las líneas contienen la información solicitada.

Dependiendo del contexto, al hacer clic con el botón derecho del ratón aparece un menú contextual con las acciones disponibles.

![010](../images/010.png)

Al hacer clic en un campo editable, se puede introducir un nuevo valor.

![011](../images/011.png)

Los campos modificados aparecen sobre un fondo magenta que desaparece tras validar los cambios.

## Los totales al pie de la tabla

![012](../images/012.png)

En la parte inferior de la tabla se muestran los totales correspondientes a las líneas mostradas o seleccionadas.

# el módulo Monitor

Se trata del módulo principal de Archiplus.

![005](../images/005.png)

Tras hacer clic en «Monitor», los comandos INFO con un historial activo se muestran en unos segundos.

![014](../images/014.png)

Al hacer clic en el botón de arriba, se puede cambiar a la visualización de todos los comandos INFO, incluso aquellos que no requieren historial o aquellos cuyo equipo está inactivo.

## Estadísticas

![016](../images/016.png)

El número de registros en «history» y «historyArch» suele corresponder al de la última copia de seguridad (se puede ver la fecha de actualización al pasar el ratón por encima de uno de los contadores). Al hacer clic en el encabezado de la columna «#All», se pueden ver inmediatamente los comandos con el historial más extenso.

![015](../images/015.png)

Al hacer clic en el botón de arriba, se puede volver a realizar un cálculo, lo que tardará unos segundos.

![017](../images/017.png)

Los totales que aparecen al pie de la tabla te permiten conocer de inmediato el tamaño de tu historial.

## Visualización

![018](../images/018.png)

Los botones de visualización permiten seleccionar los datos que se muestran

* Configuración del historial
* los cálculos
* los valores no permitidos
* visualización mediante gráficos
* las estadísticas

En función de lo que te interese, puedes activar o desactivar la sección que desees gestionar. Para no sobrecargar la pantalla de inicio de Monitor, solo se muestran los datos de identificación, configuración y estadísticas.

## Modificaciones

![020](../images/020.png)

Para modificar un dato, basta con hacer clic en el campo correspondiente e introducir un nuevo valor.

![021](../images/021.png)

Los datos modificados aparecen sobre fondo magenta.

![022](../images/022.png)

Al hacer clic con el botón derecho del ratón sobre una línea, es posible copiar su configuración o uno de sus parámetros a las líneas seleccionadas.

![023](../images/023.png)

Para comprobar los datos antes de validarlos, es posible mostrar únicamente las líneas modificadas.

![024](../images/024.png)

Tras hacer clic en el botón «Validar», los datos se actualizan y se borra el fondo de las celdas modificadas.

![025](../images/025.png)

Ten en cuenta que al hacer clic con el botón derecho del ratón sobre una línea se abre directamente la configuración avanzada de comandos de Jeedom.

## Modificaciones a partir de un archivo de Excel

![070](../images/070.png)

También es posible cargar modificaciones desde un archivo Excel o CSV haciendo clic en el botón «Importar». Este botón permite seleccionar el archivo y cargar los datos modificados en la tabla.

![071](../images/071.png)

Los datos deben tener el mismo formato que el generado por la exportación. Por lo tanto, es posible exportar los datos, modificarlos en Excel y, a continuación, cargar los cambios en la tabla.

También es posible extraer los parámetros de archivo de una copia de seguridad de Jeedom y cargar los cambios: esto permite ver rápidamente las modificaciones realizadas desde la copia de seguridad y, si fuera necesario, volver a una situación anterior.

![072](../images/072.png)

Una vez realizada la importación, es posible visualizar únicamente los datos modificados haciendo clic en el filtro «Actualizaciones». También se puede hacer clic en los botones de visualización (Configuración, Cálculos, etc.) para ver todos los datos modificables.

Para aplicar los cambios, haz clic en el botón «Validar».

## Datos modificables

Todos los datos de configuración del historial estándar de Jeedom y los específicos del complemento Archiplus se pueden modificar directamente desde Monitor.

A continuación se detallan las opciones específicas de Archiplus:

### KLV (Keep Last Value)

Permite conservar siempre al menos un registro en el historial. Consulta la siguiente pregunta frecuente para comprender cómo se utiliza esta opción [Keep Last Value](#keep-last-value).

### Uniq

Permite eliminar los valores consecutivos idénticos en historyArch (y, opcionalmente, en history). Consulta la siguiente pregunta frecuente para comprender cómo se utiliza esta opción [Uniq](#uniq-1).

### Plazo

Se trata del plazo a partir del cual se transfieren los registros de history a historyArch. De forma predeterminada en Jeedom, este parámetro es el mismo para todos los comandos. Con archiplus, este plazo se puede especificar por comando.

### Enfoque

Permite establecer el momento hasta el cual se eliminan los datos históricos, así como el momento de la transferencia de los datos de history a historyArch, con un límite de día, hora o minuto. Consulta la siguiente pregunta frecuente para comprender cómo utilizar esta opción [Plazo y intervalo](#plazo-e-intervalo).

### Pond

Permite calcular una media ponderada teniendo en cuenta el tiempo, en lugar de una media de los valores registrados durante el periodo. Consulta la siguiente pregunta frecuente para comprender cómo utilizar esta opción [Suavizado y ponderación](#suavizado-y-ponderación).

### Paquete

Define el intervalo en el que se agruparán los datos durante el suavizado. En el sistema de archivo estándar de Jeedom, este parámetro es el mismo para todos los comandos y es un múltiplo de horas. Con archiplus, se puede especificar el intervalo para cada comando y también expresar el valor en minutos (introduce el número de minutos seguido de la letra m).  Consulta la siguiente pregunta frecuente para comprender cómo utilizar esta opción [Pack](#pack-1).

### Redondeado

De forma predeterminada en Jeedom, se puede especificar el redondeo para cada comando. El complemento permite, además, especificar un redondeo diferente al suavizar los datos en historyArch. Consulta la siguiente pregunta frecuente para comprender cómo utilizar esta opción [Redondeo](#arrondi-1).

## Funciones accesibles a través del menú contextual

![026](../images/026.png)

Al hacer clic con el botón derecho en cualquier parte de una línea de la tabla, aparece el menú contextual del comando. Además de las acciones ya vistas, este permite:

* mostrar el historial en forma de gráfico  (llamada a la función estándar de Jeedom)
* mostrar los datos almacenados en las tablas history y historyArch
* borrar el historial hasta una fecha determinada
* exportar los datos históricos en formato CSV (utilizando la función estándar de Jeedom)
* actualizar las estadísticas de la línea en cuestión
* iniciar el archivo únicamente para el pedido en cuestión
* Copiar los datos de historyArch a history: Consulta la siguiente pregunta frecuente para entender cómo utilizar esta acción  [historyArch a history](#copiar-los-datos-de-historyarch-a-history)
* copiar el historial del pedido seleccionado a otro pedido

# Datos históricos

## Acceso

![027](../images/027.png)

El acceso a los datos de las tablas «history» y «historyArch» se realiza a través de:

* el menú contextual de Monitor (véase más arriba)
* seleccionar una o varias líneas y, a continuación, pulsar el botón «Datos».

![028](../images/028.png)

Los datos se muestran en una ventana modal ordenados por fecha y hora en orden descendente.

## Modificación

![029](../images/029.png)

A veces pueden aparecer valores atípicos, en este caso debido al mantenimiento de la caldera.

![030](../images/030.png)

El menú contextual permite modificar o eliminar el valor en cuestión.

![031](../images/031.png)

Tras la corrección, la visualización del historial resulta mucho más significativa.

## Eliminar

![032](../images/032.png)

También es posible eliminar varios datos históricos seleccionándolos y haciendo clic en el botón «Eliminar».

## Exportar

![033](../images/033.png)

El botón «Exportar» permite exportar los datos.

Ten en cuenta que estos archivos se pueden editar en Excel para importarlos mediante el módulo «Importar».

# El módulo Import

El módulo «Importar» permite importar datos históricos en uno o varios comandos de tipo INFO.

![035](../images/035.png)

El archivo que se va a importar debe ser de tipo Excel o CSV y debe contener al menos las tres columnas siguientes (las demás se ignorarán):

* id: ID del pedido
* fecha y hora: fecha y hora de los datos históricos en el formato AAAA-MM-DD HH:MM:SS (también se admite el formato de fecha y hora interno de Excel)
* valor: valor que se va a importar

Comprueba que los datos extraídos de los módulos Monitor o Restore tengan el formato correcto.

![034](../images/034.png)

Lo primero que hay que hacer es seleccionar el archivo que contiene los datos.

![036](../images/036.png)

Una vez completada la carga, se cargan los datos históricos del archivo.

Los datos del comando INFO se extraen de Jeedom.

Se lleva a cabo una comprobación y se detectan los datos erróneos.

![037](../images/037.png)

Es posible asignar las líneas cargadas a otro pedido seleccionando la línea o líneas en cuestión y haciendo clic en el botón «Cambiar pedido».

![038](../images/038.png)

Para importar los datos históricos a Jeedom, hay que seleccionar la línea o líneas correspondientes (en este caso, filtrando por un intervalo de fechas) y hacer clic en el botón «Importar». Las líneas con errores se ignoran.

![039](../images/039.png)

Cabe señalar que la importación se realiza mediante el método estándar cmd::addHistoryValue. Por lo tanto, se llevan a cabo los controles y procesamientos estándar de Jeedom. Las nuevas entradas se guardan en la tabla «history».

# El módulo Restore

El módulo Restore permite extraer los datos históricos de un archivo estándar de Jeedom y exportarlos para poder importarlos con el módulo Import.

Todos los procesos se realizan de forma local en el navegador web. Todos los comandos y datos históricos se cargan en la memoria del navegador. El programa se ha probado con 1,5 millones de líneas en «history» y «historyArch». El número máximo de datos que se pueden cargar depende de la memoria RAM asignada al navegador y no se puede determinar de antemano. No obstante, debería ser capaz de cargar la mayoría de las copias de seguridad en las que el historial no se haya disparado.

![040](../images/040.png)

El primer paso es recuperar la copia de seguridad en el ordenador local. Consulta la siguiente documentación sobre la gestión de copias de seguridad de Jeedom [aquí](https://doc.jeedom.com/fr_FR/core/4.5/backup).

![041](../images/041.png)

Inicia el módulo Restore y selecciona el archivo que quieras utilizar.

![042](../images/042.png)

Al cabo de unos segundos, se muestran los comandos con su historial.

![043](../images/043.png)

Puedes seleccionar los comandos que te interesen e iniciar la exportación.

![044](../images/044.png)

También puede consultar los datos históricos correspondientes y seleccionar cuáles desea exportar.

![045](../images/045.png)

En ambos casos, encontrarás un archivo de exportación que puedes utilizar para realizar una importación con el módulo Importar.


![073](../images/073.png)

Al hacer clic en los botones de visualización, se pueden mostrar los parámetros de los comandos INFO tal y como estaban en el momento de guardarlos. El filtro «Todo» permite mostrar todos los comandos INFO.

El botón «Exportar» permite generar un archivo que se podrá utilizar para cargar en el módulo Monitor los cambios de configuración respecto a la copia de seguridad.

# Preguntas frecuentes

## Mantener el último valor

En algunos casos, es necesario disponer del último valor del comando INFO.

![046](../images/046.png)

Tomemos el caso de una caldera en la que se lee periódicamente el contador de gas destinado a la calefacción.

![047](../images/047.png)

Un escenario que se ejecuta cada hora permite calcular el consumo horario calculando la diferencia entre el valor registrado en el historial al inicio y al final de la hora. Para ello, basta con disponer de un historial de un día.

Sin embargo, cuando termina la temporada de calefacción, el historial del contador de calefacción desaparece y ya no está disponible para calcular el primer consumo horario durante el primer encendido de la temporada siguiente.

Al activar la opción «Keep Last Value» se puede solucionar este problema sin tener que recurrir a trucos de programación ni mantener un historial de un año.

## Uniq

Jeedom permite evitar duplicados en la tabla «history» gracias a la opción «Repetir valores idénticos», que está desactivada por defecto.

Sin embargo, hay varias situaciones en las que los valores consecutivos idénticos no se ignoran:

  * si el subtipo del comando es «Binario» u «Otro»
  * si la actualización se realiza con el método cmd::event y no con eqLogic::checkAndUpdateCmd. Muchos complementos siguen funcionando con el método cmd::event, que es más antiguo y, por lo tanto, no elimina los duplicados.

Al archivar, si no se aplica el suavizado, los datos del historial se transfieren directamente a historyArch y, por lo tanto, se copian los duplicados.

Al activar la opción Uniq, se eliminan los duplicados en historyArch durante el archivado específico de archiplus.

Además, si el complemento está configurado para no copiar los registros de «history» a «historyArch», los duplicados de «history» también se eliminarán.

## Plazos y alcance

De forma predeterminada, el momento a partir del cual se eliminan los datos de history y historyArch viene definido por el parámetro «Purgar historial», expresado en horas. En la configuración global de Jeedom se establece un valor por defecto.

Así, con una purga definida en 7 días, si el archivado se inicia el 20/01/2025 a las 05:11:21, se eliminarán los registros de history y historyArch hasta el 13/01/2025 a las 05:11:21.

El parámetro «Encuadre» específico de Archiplus permite fijar con mayor precisión el momento de la purga. Así, en el ejemplo anterior, el momento de la purga será:

* el 13/01/2025 a las 05:11:21 si no se ha definido ningún marco
* el 13/01/2025 a las 05:11:00, con un primer plano del último minuto
* el 13/01/2025 a las 05:00:00, con un enfoque en la última hora
* el 13/01/2025 a las 00:00:00, centrándose en el último día

El «Plazo antes del archivo» (en horas) permite determinar a partir de qué momento se transfieren los registros del historial a historyArch (con o sin consolidación). Por defecto, se define de forma global y, por lo tanto, es idéntico para todos los controles.

El sistema de archivo específico de archiplus permite definir un plazo concreto para cada comando INFO y utilizar el esquema que se ha visto anteriormente. Así, con un plazo de 2 horas, el momento de la transferencia de history a historyArch será:

* el 20/01/2025 a las 03:11:21 si no se ha definido ningún marco
* el 20/01/2025 a las 03:11:00, con un primer plano del último minuto
* el 20/01/2025 a las 03:00:00, con un enfoque en la última hora
* el 20/01/2025 a las 00:00:00, centrándose en el último día, independientemente de la hora del día en que se inicie el archivado

Ten en cuenta que el momento de la purga no puede ser posterior al momento de la transferencia de history a historyArch, por lo que se ajustará automáticamente.

![048](../images/048.png)

Se pueden ajustar estos parámetros si, por ejemplo, se desea un historial detallado de un periodo corto (en este caso, un máximo de 36 horas) sin necesidad de un archivo consolidado. De este modo, se evita la transferencia del historial a historyArch, que no aporta nada.

## Suavizado y ponderación

El suavizado se aplica al copiar los datos de «history» a «historyArch». El proceso de archivado tiene en cuenta todos los datos de «history» según el intervalo definido (por defecto, una hora) y conserva un único valor calculado según el modo de suavizado. Hay tres modos posibles:

* mínimo: el menor de los valores contenidos en el intervalo
* máximo: el mayor de los valores contenidos en el intervalo
* media: la media de los valores contenidos en el intervalo

Cabe señalar que el archivo estándar no tiene en cuenta el valor del comando al inicio del intervalo y calcula la media de los valores presentes en el intervalo, lo que puede distorsionar significativamente el resultado.

El proceso específico de archivo de archiplus ofrece una opción «Pond» que permite corregir este fenómeno y calcular un resultado exacto para el intervalo considerado.

Esto se ilustra en el ejemplo siguiente.

![050](../images/050.png)

Consideremos dos comandos con las siguientes configuraciones.

![049](../images/049.png)

Tienen las mismas entradas en la tabla «history»

![051](../images/051.png)

Tras el archivado, las entradas en historyArch son diferentes

![052](../images/052.png)

Con el archivo estándar, se tiene en cuenta la media de los valores del periodo.

Con el archivado específico de Archiplus, se calcula la media ponderada del periodo. Cabe señalar también que se añade una entrada en «history» para conocer, en el siguiente archivado, el valor inicial del periodo (sin esta entrada, se recuperaría la media del último periodo, lo que distorsionaría el cálculo).

## Paquete

De forma predeterminada en Jeedom, el intervalo (denominado «paquete» en Jeedom) sobre el que se puede aplicar el suavizado se define en horas y es el mismo para todos los comandos INFO.

Sin embargo, es posible que se desee un intervalo más corto y poder especificarlo para un comando INFO concreto.

![055](../images/055.png)

![054](../images/054.png)

En el caso de una batería, puede bastar con mantener un valor al día durante un periodo prolongado.

![057](../images/057.png)

![056](../images/056.png)

En el caso de un termómetro, un valor cada cuarto de hora puede resultar más útil que uno cada hora.

Para indicar minutos, introduce en el campo «Pack» el número de minutos que desees seguido de «m», por ejemplo, «15m».

## Redondeado

De forma predeterminada, Jeedom permite especificar el número de decimales de un valor de comando INFO.

Para algunos comandos, puede resultar interesante disponer de un valor preciso durante un breve periodo de tiempo y, posteriormente, de uno menos preciso. Por ejemplo, conocer la temperatura exterior exacta es útil en ese momento, pero deja de ser necesario al cabo de varios días.

![064](../images/064.png)

El comando anterior está configurado para conservar un historial con un decimal durante una semana y un historial sin decimales a partir de entonces.

![065](../images/065.png)

Antes del archivo, hay 7 registros en el historial con temperaturas entre 7,7 °C y 8,3 °C.

![066](../images/066.png)

Tras el archivado, los 7 valores introducidos se redondean a 8 °C y la opción «Uniq» permite conservar solo uno de ellos.

## Copiar los datos de historyArch a history

Una vez instalado Archiplus, es posible que quieras consolidar los historiales existentes.

![060](../images/060.png)

![058](../images/058.png)

Por ejemplo, para este comando, un historial con intervalos de 10 minutos sería suficiente y reduciría considerablemente el número de registros en historyArch.

![059](../images/059.png)

Tras modificar la configuración, se pueden transferir las entradas de historyArch a history.

![061](../images/061.png)

Una vez realizada esta actualización, se puede iniciar un proceso de archivado mediante este comando INFO (o esperar a que el archivado se inicie automáticamente por la noche).

![063](../images/063.png)

![062](../images/062.png)

Tras el archivado, el número de registros se reduce considerablemente y la visualización del historial es mucho más rápida.

## Usar Archiplus en PHP

Es posible llamar a las funciones de archivo y procesamiento de historiales de archiplus directamente desde un escenario o una función PHP.

![053](../images/053.png)

En este caso, las funciones de Archiplus se utilizan en un escenario para cargar el historial de un pedido e iniciar el archivo del mismo.

`require_once dirname(__FILE__) . '../../../plugins/archiplus/core/class/archiplus.class.php';`

Esta línea permite cargar el código de las funciones de Archiplus. Puede que sea necesario adaptar la ruta para que apunte a la clase del complemento.

Las funciones disponibles se pueden encontrar en el código de la clase archiplus. Las principales son:

* `archive($_cmd_id = '')`: inicia el archivo de un pedido o de todos los pedidos si no se especifica ningún parámetro
* `History_purge($_cmd_id, $_date='')`: elimina el historial de un comando hasta una fecha y hora determinadas (o todo el historial si no se especifica un segundo parámetro)
* `addHistoryValue($_cmd_id, $_datetime, $_value)`: añade una entrada (o sustituye la existente) en el historial llamando a la función estándar de Jeedom
* `historyArch2history($_cmd_id, $_date_start = '', $_date_end = '')`: transfiere los registros de historyArch a history
  
Por supuesto, es posible utilizar las funciones disponibles en la clase history.class.php tras haber realizado la declaración `require_once` necesaria.

# Los registros

Si el nivel de registro en la configuración del complemento está establecido como mínimo en «Info», los distintos eventos relacionados con Archiplus se registrarán en el registro de Archiplus de Jeedom. Se puede acceder a él directamente mediante el botón «Registro» presente en los distintos módulos de Archiplus.

![068](../images/068.png)

Al realizar el archivado, se muestran los parámetros generales de archivado de Jeedom.

![067](../images/067.png)

A continuación, se detallan, para cada comando, las operaciones realizadas y el número de registros en history y historyArch antes y después de dicho comando.

![069](../images/069.png)

Es posible visualizar el registro de un comando concreto indicando su número, precedido de los caracteres «-» y un espacio, en el campo de búsqueda.

# Traducción

La interfaz, los mensajes que aparecen en los registros y la documentación están traducidos a los cinco idiomas compatibles con Jeedom (gracias a @mips por el desarrollo de ga-translation y docs-translations). Si detectas algún error de traducción, puedes abrir una solicitud de asistencia y, si es posible, adjuntar el archivo de traducción corregido (que se encuentra en el directorio core/i18n del complemento).

# Opiniones

![archiplus_opiniones](../images/archiplus_avis.png)

Si te gusta este plugin, te agradeceríamos que dejaras una valoración y un comentario en Jeedom Market, siempre nos hace ilusión: <https://jeedom.com/market/index.php?v=d&p=market_display&id=xxxx#>
