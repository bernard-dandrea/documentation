
<!--  
Última modificación: 26/07/2026 18:45:10
-->


# Complemento EcoNetatmo

Complemento que permite recopilar los datos de consumo de los contadores ecológicos Legrand tipo Drivia con NetAtmo (ref. 41203x).

Este complemento se ha desarrollado a partir del complemento estándar netatmoWeather.

Este complemento utiliza las API proporcionadas por Netatmo (consulta el siguiente enlace <https://dev.netatmo.com/apidocumentation/control>).

# Recuperación de los datos de inicio de sesión

Para acceder a los datos de tu Ecocompteur, debes disponer de un client\_id y un client\_secret generados en la página web <https://dev.netatmo.com>.

Si aún no lo has hecho, crea una cuenta en <https://auth.netatmo.com/fr-fr/access/signup?next_url=https%3A%2F%2Fdev.netatmo.com%2Fbusiness-showcase>

![aplicaciones](../images/apps.png)

Una vez que hayas iniciado sesión, ve al menú de aplicaciones ( <https://dev.netatmo.com/apps/> ) y haz clic en «Create».

![aplicación](../images/app.png)

Rellena el formulario y haz clic en «Guardar».

![secreto](../images/secret.png)

Se han generado el «ID de cliente» y el «secreto de cliente». Puedes utilizarlos para configurar el complemento.


# Recuperación de tokens

Los tokens permiten acceder a tus datos en los servidores de Netatmo (consulta el estándar de autorización OAuth 2.0).

Se pueden generar directamente en la página de la aplicación.

![generar_token](../images/generate_token.png)

Selecciona el ámbito «read_magellan» y haz clic en «Generate Token».

![tokens](../images/tokens.png)

Una vez que hayas autorizado el acceso a tus datos, se generarán los tokens.

# Configuración del complemento

Una vez instalado el complemento, hay que activarlo e introducir tus datos de inicio de sesión de Netatmo:

![configuración](../images/configuration.png)

-   **ID de cliente**: tu ID de cliente (consulta la sección de configuración)
-   **Cliente secreto**: tu cliente secreto (véase la sección de configuración)
-   **Token de acceso**: token que permite acceder a tus datos en los servidores de Netatmo
-   **Token de actualización**: token que permite actualizar el token de acceso

La gestión de los tokens la lleva a cabo el complemento. En caso de que estos dejen de ser válidos (consulta los registros), por ejemplo, tras un largo periodo de inactividad, habría que generar otros nuevos y actualizar la configuración del complemento con los nuevos tokens.

También puedes configurar si se utiliza un cron independiente. Esto permite que, si el cron del plugin se bloquea, no se bloqueen los demás crons, y que el plugin no se vea bloqueado por otros crons ejecutados por otros plugins.

![registro](../images/log.png)

Puedes activar el nivel de registro «Debug» para realizar un seguimiento de la actividad del complemento e identificar posibles problemas.

# Configuración de los dispositivos

Se puede acceder a la configuración de los dispositivos Netatmo desde el menú del complemento (menú Complementos, Energía y, a continuación, EcoNetAtmo):

![sincronización](../images/synchronisation.png)

Haz clic en «Sincronización» para iniciar la creación de los dispositivos. Se utiliza la API /homesdata para recuperar la información (véase <https://dev.netatmo.com/apidocumentation/control#homesdata>).

![equipos](../images/equipements.png)

Se han creado los contadores de las líneas eléctricas. Hay un equipo por línea.

![equipos](../images/equipement.png)

En la pestaña «Equipos» encontrarás toda la configuración de tus equipos:

-   **Nombre**: nombre de tu contador (este dato se toma de la configuración de Netatmo)
-   **Objeto principal**: indica el objeto principal al que pertenece el equipo
-   **Categoría**: indica la categoría de Jeedom del equipo
-   **Activar**: permite activar tu equipo
-   **Visible**: lo muestra en el panel de control
-   **ID del módulo**: indica el identificador único del dispositivo en Netatmo
-   **Tipo de consumo**: indica el tipo de tu dispositivo en Netatmo
-   **Tipo de fuente**: indica la fuente de energía de tu equipo en Netatmo
-   **Icono**: permite seleccionar un tipo de icono para tu equipo en el panel de configuración
  
El botón «Importar contadores» permite crear los comandos correspondientes al equipo. Esto se realiza al crear el equipo y solo resulta útil si has eliminado un comando.

![comandos](../images/comandos.png)

En la pestaña «Comandos» encontrarás la lista de comandos (estos se generan al crear el dispositivo).

El comando «Refresh» permite iniciar la recuperación inmediata de los valores de los contadores. Por defecto, se inicia una recuperación cada 10 minutos.

Los demás comandos corresponden a los contadores alimentados por Netatmo (véase la API /getmesure <https://dev.netatmo.com/apidocumentation/control#getmeasure>). Para cada uno de ellos, además de los valores habituales de Jeedom, encontramos:

-   el nombre que aparece en el panel de control
-   el logicalID que corresponde al «tipo» en la API de Netatmo
-   la posibilidad de activar o desactivar la lectura del contador
-   el periodo correspondiente a «scale» en la API de Netatmo (para el que se desean recuperar los datos; solo se muestran los valores permitidos por la API de Netatmo)

# Widget

![widget](../images/widget.png)

Este es el widget estándar.

# Preguntas frecuentes

>**¿Cuál es la frecuencia de actualización?**
>
>El complemento recoge la información cada 10 minutos. Sin embargo, el contador ecológico envía sus lecturas aproximadamente cada 3 horas, por lo que se puede observar este desfase en la recogida de datos.

>**¿Puedo recoger los contadores de gas y agua?**
>
>El complemento es capaz de hacerlo. Por desgracia, la API de Netatmo no especifica qué «tipo» hay que utilizar para recuperar estos valores. Se ha enviado una consulta al equipo encargado del desarrollo de la API, pero aún no se ha recibido respuesta.

# Traducción

La interfaz, los mensajes que aparecen en los registros y la documentación están traducidos a los cinco idiomas compatibles con Jeedom (gracias a @mips por el desarrollo de ga-translation y docs-translations). Si detectas algún error de traducción, puedes abrir una solicitud de asistencia y, si es posible, adjuntar el archivo de traducción corregido (que se encuentra en el directorio core/i18n del complemento).

# Opiniones

![EcoNetatmo_opiniones](../images/EcoNetatmo_avis.png)

Si te gusta este plugin, te agradeceríamos que dejaras una valoración y un comentario en Jeedom Market, siempre nos hace mucha ilusión: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4413#>
