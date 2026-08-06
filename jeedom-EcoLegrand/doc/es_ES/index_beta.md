
<!--  
Última modificación: 27/07/2026 15:27:46
-->


# Complemento EcoLegrand

Complemento que permite recuperar los datos de los contadores ecológicos Legrand de antigua generación (referencia 412000).

A diferencia de los nuevos contadores ecológicos, cuyos datos solo son accesibles a través de la nube, se puede acceder a los antiguos contadores ecológicos mediante una interfaz web local. En concreto, se puede visualizar directamente el consumo instantáneo, algo que no es posible con los nuevos contadores ecológicos (hay que consultar los datos directamente en el contador).

Los contadores ecológicos 412000 dejaron de comercializarse en 2020, pero siguen siendo muy interesantes en comparación con la versión actual.

La comunicación entre el complemento y el contador ecológico se lleva a cabo mediante la recuperación de datos de archivos JSON definidos por el usuario. El propio usuario define en el archivo JSON los datos que desea recuperar.

La función básica del complemento es la recuperación de datos de los contadores ecológicos. Su análisis debe realizarse por otros medios (virtuales, escenarios, etc.) y requiere un cierto dominio de Jeedom para poder manipular los datos.

# Instalación y configuración del contador ecológico EcoLegrand

Para que el complemento funcione correctamente, es necesario que el contador ecológico esté operativo y sea accesible a través de la interfaz web.

El complemento se ha probado con la versión 3.0.17, que es la última publicada y ya no se actualizará, ya que este contador ecológico ya no recibe mantenimiento.

# Definición de los datos que se deben recuperar en un archivo JSON

Los datos que hay que recuperar se definen en un archivo JSON que debe copiarse en el contador ecológico.

{   
«contador_C1»:~LG536 2 12724$,
«contador_C2»:~LG536 4 12 724 $,
«contador_C3»:~LG536 6 12 724 $,
«contador_C4»:~LG536 8 12 724 $,
«contador_C5»:~LG536 10 12 724 $,
«Contador_EF»:~LG538 0 12 907 $,
«Contador_EC»:~LG538 1 12 907 $
}

El archivo JSON tiene el formato que se muestra arriba. Hay una línea por cada dato que se desea recuperar (ten en cuenta que no debes poner una coma en la última línea y que debes utilizar comillas dobles).

Cada línea incluye el nombre del dato y la referencia interna definida en el contador ecológico. El archivo al que se accede mediante el enlace <https://github.com/bernard-dandrea/documentation/blob/main/jeedom-EcoLegrand/doc/fr_FR/JSON_codes.txt> ofrece una lista no exhaustiva de las referencias que se pueden utilizar.

Puedes consultar el siguiente foro <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=20> para obtener más información.

# Copia del archivo JSON en el contador ecológico

La copia se realiza mediante el protocolo FTP. Se puede utilizar el programa FileZilla.

![FileZilla_Connect](../images/FileZilla_Connect.png)

Inicia sesión indicando la dirección IP y las credenciales de acceso (por defecto, admin / password).

![FileZilla_SYS](../images/FileZilla_SYS.png)

Accede al directorio SYS.

![FileZilla_COPY](../images/FileZilla_COPY.png)

Copia el archivo JSON. Ten en cuenta que el nombre debe ser bastante corto; de lo contrario, no se podrá copiar.

En el directorio SYS se encuentran los archivos HTML que utiliza el contador ecológico. Si los analizas, podrás encontrar las referencias a las variables que te interesan.

# Problema con los contadores de energía

El foro anterior explica muy bien el problema que se plantea con los contadores de energía (los contadores de impulsos no se ven afectados).

Parece que el software del contador ecológico gestiona internamente estos contadores con variables de tipo float 32. Estas tienen una precisión de aproximadamente 7 decimales.

Estos contadores se actualizan cada segundo y se gestionan en kWh con seis decimales.

Por ello, cuando se superan los 10 kWh, se empieza a perder precisión, sobre todo si el consumo en la línea en cuestión es bajo. Esto resulta muy perjudicial cuando se superan los 100 kWh.

Para solucionar este problema, el complemento puede poner a cero los contadores a partir de un umbral definido por el usuario (normalmente de 1 a 10 kWh). El complemento gestiona el desfase y proporciona un valor acumulado del contador. Ten en cuenta que este reinicio del contador interno puede alterar las estadísticas proporcionadas por el ecocontador.

# Instalación del complemento

Una vez instalado el complemento, hay que activarlo.


![Configuración](../images/configuration.png)

También puedes configurar si se utiliza un cron independiente. Esto permite que, si el cron del plugin se bloquea, no se bloqueen los demás crons, y que el plugin no se vea bloqueado por otros crons ejecutados por otros plugins.

Puedes activar el nivel de registro «Debug» para realizar un seguimiento de la actividad del complemento e identificar posibles problemas.

# Configuración de los dispositivos

Se puede acceder a la configuración de los dispositivos desde el menú del complemento (menú Complementos, Energía y, a continuación, Ecocompteur Legrand).

Haz clic en «Añadir» para configurar un contador ecológico.

![Equipamiento](../images/Equipement.png)

Indica la configuración del contador ecológico:

-   **Nombre**: nombre del contador ecológico
-   **Objeto principal**: indica el objeto principal al que pertenece el equipo
-   **Categoría**: indica la categoría de Jeedom del equipo
-   **Activar**: permite activar el equipo
-   **Visible**: lo muestra en el panel de control
-   **Dirección IP**: IP del dispositivo
-   **Archivo JSON**: archivo JSON que contiene la definición de los datos que se van a recuperar

Los siguientes botones permiten realizar las siguientes funciones:

-   **Acceder al contador ecológico**: permite iniciar sesión en la web del contador ecológico
-   **Probar el JSON**: permite probar el archivo JSON y comprobar si los parámetros devueltos son correctos
-   **Crear contadores**: genera los comandos de información correspondientes a los contadores

# Comandos asociados a los equipos

![Controles](../images/Commandes.png)

Por defecto, se crean dos comandos:

- Última actualización: indica cuándo se actualizó por última vez la información del contador ecológico
- Actualizar: permite forzar la recuperación de los contadores. Una tarea programada (cron) ejecuta la actualización cada minuto.

Se crea un comando de información para cada uno de los contadores. Para cada uno de ellos, además de los campos habituales de Jeedom, encontramos:

- la casilla de actualización que permite solicitar o no la actualización del contador
- el umbral, que es el valor a partir del cual se pone a cero el contador
- el comando que pone a cero el contador
- el «offset», que es el valor acumulado del contador en el momento de la puesta a cero
- el valor actual del contador (desviación + valor del contador en el ecocontador)

¿Cuál es el comando que permite reiniciar los contadores y del tipo «http://192.168.1.xxx/wp.cgi»?wp=536+X+12724+-1+-1+4+0.0, es decir, wp.cgi? seguido de las referencias de los contadores y de valores fijos; por ejemplo, wp=536+2+12724+-1+-1+4+0.0 para el contador_C1. Consulta el foro <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=120> para obtener más información.

Para los campos no numéricos, cambia el tipo de campo de «Numérico» a «Otro» (el umbral y el desplazamiento no tienen sentido en este caso).

# Widget

![Widget](../images/Widget.png)

Este es un ejemplo de widget. Ten en cuenta que debes indicar tú mismo las unidades en el comando.

# Aprovechamiento de los datos

Mediante escenarios, ya sean virtuales o procedimientos PHP, es posible generar tus propios indicadores a partir de los contadores.

![potencia](../images/puissance.png)

Por ejemplo, se puede generar un informe de potencia basado en el cálculo de la potencia media entre dos mediciones.

![consumo_diario](../images/conso_jour.png)

O generar informes diarios de consumo eléctrico.

# Preguntas frecuentes

Puede ocurrir que el archivo JSON enviado por el contador ecológico no se pueda descodificar.

![json_error](../images/json_error.png)

En ese caso, se muestra un mensaje en el registro.

![json_lint](../images/json_lint.png)

Para localizar el origen del error, recupera del registro el archivo JSON devuelto por el contador ecológico y pruébalo en la página web <https://jsonlint.com/>.

En este caso, el error se debe a que la rutina de conversión no admite el 0 inicial en la entrada «Linky_Conso»:024795944.

Esto se puede solucionar poniendo el valor 024795944 entre comillas.

Para ello, modifica el archivo de definición de los datos que se van a recuperar y añade comillas en la entrada correspondiente:

«Linky_Conso»:~LG526 1 12005$, --> «Linky_Conso»:«~LG526 1 12005$»,

La cadena «024795944» se considerará entonces como una cadena y ya no habrá ningún problema durante la conversión.

# Opiniones

![EcoLegrand_opiniones](../images/EcoLegrand_avis.png)

Si te gusta este plugin, te agradeceríamos que dejaras una valoración y un comentario en Jeedom Market, siempre nos hace mucha ilusión: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4430#>
