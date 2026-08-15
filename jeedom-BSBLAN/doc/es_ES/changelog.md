# Registro de cambios del complemento BSBLAN

# 28/07/2026

- Cifrado de los códigos de acceso en la base de datos
- Mejora de la gestión de las tareas programadas
- Posibilidad de ejecutar el cron en el motor de tareas
- Limpieza de código

# 05/01/2026

- Sustitución de «event» por «checkAndUpdateCmd» para evitar la repetición de valores en el historial
- Traslado de la documentación a un repositorio de GitHub independiente para poder actualizarla sin que ello implique una actualización del complemento

# 27/01/2025

- Gestión de órdenes de actualización mediante JSON o URL/S (véase la documentación)

# 10/11/2024

- Actualización de la documentación

# 07/11/2024

- Conversión de los métodos cron a estáticos para evitar errores en PHP 8

# 06/08/2024

- Posibilidad de volver a enviar varias veces un pedido que no se haya podido tramitar

Tras la actualización a Debian 11, me di cuenta de que se producían tiempos de espera tras enviar comandos al BSBLAN (esto no ocurría en Debian 10 y no sé dónde buscar para solucionar el problema a nivel del sistema operativo). Al volver a enviar el comando, este suele ejecutarse sin problemas. Por eso he añadido en cada dispositivo una opción llamada «Número de intentos» que permite enviar el comando varias veces.

# 28/04/2024

- Actualización menor de la documentación

# 25/02/2024

- Ampliación de la longitud de los nombres de los comandos de 40 a 100 caracteres
- Actualización de la documentación

# 28/12/2023

- Se ha añadido un comando «refresh» (que se crea al guardar el equipo)
  
# 21/10/2023

- Actualización del mensaje de depuración
- Actualización del índice y del registro de cambios para la versión beta

# 01/08/2023

- Incorporación de un tiempo de espera para las solicitudes HTTP

# 10/07/2023

- Carga inicial

