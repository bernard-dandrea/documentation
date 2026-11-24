# Registro de cambios del complemento EcoLegrand

# 24/11/2026

- Comprueba que el valor devuelto por el contador ecológico sea numérico

# 22/11/2026

- Traducción del complemento y de la documentación a en_US, es_ES, de_DE, it_IT y pt_PT
- Mejora de los mensajes en los registros
- Cifrado de los códigos de acceso en la base de datos
- Migración a Vanilla JS
- Limpieza del código y optimizaciones
- Simplificación de la gestión de la documentación en GitHub

# 27/07/2026

- Posibilidad de configurar un cron independiente en el motor de tareas

# 01/01/2026

- Modificaciones para convertir los valores obtenidos del contador ecológico a tipo «float» y evitar errores al sumarlos en PHP 8 (gracias a Michel_F)
- Traslado de la documentación a un repositorio de GitHub independiente para poder actualizarla sin que ello implique una actualización del complemento

# 28/03/2025

- Recuperación de campos no numéricos

# 07/11/2024

- Conversión de los métodos cron a estáticos para evitar errores en PHP 8
- Mejora de la documentación

# 25/02/2024

- Actualización de la documentación

# 21/12/2023

- Mensaje de error en el registro si no se puede descodificar el JSON enviado por el contador ecológico (véase la sección de preguntas frecuentes)
  
# 24/08/2023

- Corrección de un error al crear comandos

# 14/08/2023

- Incorporación de un registro al crear un contador

# 05/08/2023

- Carga inicial
