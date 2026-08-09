<!--  
Última modificación: 28/07/2026 15:35:23
-->


# Complemento OZW

Complemento que permite conectarse con las centrales de comunicación SIEMENS del tipo OZW.

Las centrales de comunicación OZW se utilizan para comunicarse con las tarjetas que controlan numerosas calderas, bombas de calor y otros dispositivos industriales. Estas cuentan con un servidor web integrado desde el que se pueden controlar los dispositivos conectados a ellas.

Hay dos modelos cuyo funcionamiento es prácticamente idéntico:

- OZW672 para la comunicación con los dispositivos directamente a través del bus LPB, BSB
- OZW772 para la comunicación con los dispositivos a través del protocolo KNX

La comunicación entre el complemento y el OZW se realiza a través de las API web proporcionadas por SIEMENS, que permiten simular las interacciones que normalmente se llevan a cabo en el servidor web.

Este complemento es una importante evolución del complemento OZW672 (véase https://github.com/NextDom/plugin-ozw672), que ya no recibe mantenimiento y no funciona en la versión actual de Jeedom.

# Instalación y configuración del controlador OZW

Para la instalación de la central de comunicación WEB, consulte la documentación correspondiente de SIEMENS.

![OZW_WEB_ACCESS](../images/OZW_WEB_ACCESS.png)

Activar el acceso a las API web (menú Inicio > 0.5 OZWx72.01 > Ajustes > Comunicación > Servicios).

El complemento se ha probado con la versión 12 del servidor web. En principio, el complemento debería funcionar con versiones anteriores, ya que las llamadas a las API son bastante básicas y deben existir desde hace muchas versiones.

![OZW_inicio](../images/OZW_accueil.png)

Una vez realizada la instalación, debería aparecer una página web similar a esta.

En esta configuración hay dos dispositivos:

-   el primero muestra una tarjeta LMS14 que controla una caldera
-   el segundo representa la central de comunicaciones OWZ672 y permite su configuración

![OZW_dispositivo](../images/OZW_device.png)

Se puede acceder a los distintos puntos de datos definidos para el mapa. Es posible consultarlos y, si es necesario, modificarlos.

En las API proporcionadas por SIEMENS, los puntos de datos deben especificarse mediante su referencia web, que se puede encontrar en la interfaz web.

![OZW_punto de datos_referencia](../images/OZW_datapoint_reference.png)

Para encontrarla, sitúate en la línea correspondiente e inicia la inspección del elemento (por lo general, haz clic con el botón derecho del ratón y selecciona «Inspeccionar»). En el código correspondiente, encontrarás un número en la instrucción «openDialog('xxx')» o «id='dpxxx'» que indica la referencia WEB, 591 en el ejemplo anterior.

![OZW_ID_menu](../images/OZW_ID_menu.png)

Del mismo modo, puede ser necesario el ID de un menú, que se encuentra de la misma manera: 590 en el ejemplo anterior.

# Configuración del complemento

Una vez instalado el complemento, hay que activarlo.

![Configuración](../images/OZW_configuration.png)

También puedes configurar si se utiliza un cron independiente. Esto permite que, si el cron del plugin se bloquea, no se bloqueen los demás crons, y que el plugin no se vea bloqueado por otros crons ejecutados por otros plugins.

Puedes activar el nivel de registro «Debug» para realizar un seguimiento de la actividad del complemento e identificar posibles problemas.

# Configuración de los dispositivos

Se puede acceder a la configuración de los dispositivos desde el menú del complemento (menú Complementos, Objetos conectados y, a continuación, OZW).

Haz clic en «Añadir» para configurar el OZW.

![OZW_Equipamiento_OZW](../images/OZW_Equipement_OZW.png)

Indica la configuración del OZW:

-   **Nombre**: nombre de la OZW
-   **Objeto principal**: indica el objeto principal al que pertenece el equipo
-   **Categoría**: indica la categoría de Jeedom del equipo
-   **Activar**: permite activar el equipo
-   **Visible**: lo muestra en el panel de control
-   **Dirección IP**: IP del dispositivo
-   **Nombre de usuario y contraseña**: datos de acceso al servidor web
-   **Duración de una sesión**: periodo tras el cual se renueva el identificador de sesión
-   **Icono**: permite seleccionar un tipo de icono para el dispositivo en el panel de configuración

Una vez guardado el OZW, se activan los siguientes botones:

-   **Acceder a la OZW**: permite iniciar sesión en la OZW a través de la web
-   **Importar dispositivos**: permite importar los equipos correspondientes a los dispositivos conectados al OZW.

![OZW_Equipamiento_OZW_dispositivos](../images/OZW_Equipement_OZW_devices.png)

En el ejemplo anterior, tras importar los dispositivos, encontramos:

- el OZW672 como equipo principal
- el OZW672.01 como dispositivo
- la tarjeta LMS14 que controla la caldera

![OZW_Equipamiento_OZW_dispositivo](../images/OZW_Equipement_OZW_device.png)

Es posible asociar un icono específico al dispositivo. También se puede personalizar un icono de tipo «perso» añadiendo la imagen correspondiente (por ejemplo, «perso1.png» para el icono «perso1») en el directorio «plugin_info» del complemento.

# Comandos asociados a los equipos

![OZW_Controles](../images/OZW_Commandes.png)

Para el OZW, se crean dos comandos de tipo «info»:

- Estado: igual a 1 cuando se ha establecido la comunicación con el servidor web; 0 en caso contrario
- SessionID: ID utilizado por las API web

![OZW_Comandos_device_initial](../images/OZW_Commandes_device_initial.png)

Para los dispositivos conectados al OZW, se crean dos comandos:

- Última actualización: comando de tipo «info» que indica cuándo se actualizó por última vez la información del dispositivo
- Refresh: comando de tipo acción que permite actualizar todos los puntos de datos para los que está activada la actualización

![OZW_Importar_Menú_principal](../images/OZW_Importer_Menu_principal.png)

El botón «Importar comandos principales» de la pestaña «Equipos» permite importar todos los puntos de datos del menú denominado «móvil». Este menú está disponible en la aplicación para Android proporcionada por SIEMENS y no está disponible para todos los dispositivos. La creación de los comandos puede tardar varios minutos. Una vez completada la operación, aparecerán los principales puntos de datos del dispositivo definidos como comandos de tipo «info».

![OZW_import_menu_específico](../images/OZW_import_menu_specifique.png)

Del mismo modo, el botón «Importar menú» de la pestaña «Equipos» permite importar todos los puntos de datos de un menú específico. Para ello, hay que indicar la referencia web del menú.


![OZW_botones_importar_pedido](../images/OZW_boutons_import_commande.png)

En la pestaña «Comandos», están disponibles los siguientes botones:

- Importar un punto de datos: permite crear un comando de información para un punto de datos específico
- Añadir una acción: permite modificar el valor del punto de datos (cuando el servidor web lo permita)
- Añadir un comando «refresh»: permite forzar la recuperación del valor del punto de datos

**Atención**: introduce correctamente la referencia WEB del punto de datos y no el número de línea que aparece en la línea del punto de datos.

# Análisis de los campos del pedido

![OWZ_Análisis_de_control](../images/OWZ_Analyse_commande.png)

Para cada comando relacionado con un punto de datos, además de los campos habituales de Jeedom, se encuentran:

- el LogicalID:
  - para comandos de tipo «info», igual a la referencia WEB del punto de datos
  - para los comandos de acción, igual a «A_» seguido de la referencia WEB del punto de datos
  - para los comandos de actualización, igual a «R_» seguido de la referencia WEB del punto de datos
- la casilla de selección que permite solicitar o no la actualización del punto de datos
- el campo «scan», que indica la frecuencia de actualización del punto de datos

# Widget

![OZW_widget](../images/OZW_widget.png)

Este es un ejemplo de widget. Se puede modificar el nombre de los comandos para que coincida con el número de línea indicado en el servidor web.

# Traducción

La interfaz, los mensajes que aparecen en los registros y la documentación están traducidos a los cinco idiomas compatibles con Jeedom (gracias a @mips por el desarrollo de ga-translation y docs-translations). Si detectas algún error de traducción, puedes abrir una solicitud de asistencia y, si es posible, adjuntar el archivo de traducción corregido (que se encuentra en el directorio core/i18n del complemento).

# Opiniones

![OZW_opinión](../images/OZW_avis.png)

Si te gusta este plugin, te agradeceríamos que dejaras una valoración y un comentario en Jeedom Market, siempre nos hace mucha ilusión: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4414#>
