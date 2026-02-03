**AgendaBot - Workflow n8n con Telegram y Google Sheets**
- Descripción general
Este proyecto implementa un bot de Telegram integrado con n8n para gestionar una agenda personal. El bot permite a los usuarios interactuar mediante un menú, agendar citas, consultar información y navegar entre diferentes módulos como tareas, recordatorios, hábitos y reportes.

El estado de cada usuario y el progreso de sus acciones se almacenan en Google Sheets, funcionando como una base de datos ligera para manejar sesiones y flujos conversacionales.

- Tecnologías usadas

n8n → Orquestador de workflows

Telegram Bot API → Interfaz de usuario

Google Sheets → Almacenamiento de sesiones y datos

Webhooks (n8n + ngrok) → Exposición pública del bot

- Estructura del flujo
1️⃣ Telegram Trigger

Nodo principal que recibe los mensajes del usuario desde Telegram.

Escucha mensajes entrantes

Extrae:

Texto del mensaje

ID del chat

Nombre del usuario

2️⃣ Menú Principal

El bot muestra un menú inicial:

0. Ayuda  
1. Agenda (citas)  
2. Tareas  
3. Recordatorios  
4. Hábitos  
5. Listas  
6. Reportes  
7. Configuración  
8. Administrador  


El usuario debe escribir solo el número.

3️⃣ Switch (Router de opciones)

Se usa el nodo Switch para:

Detectar qué número envió el usuario

Redirigir al flujo correspondiente

Enviar mensaje de error si la opción no existe

4️⃣ Gestión de sesiones (Google Sheets)

Se usa una hoja de Google Sheets llamada sessions para guardar:

telegram_user

pantalla_actual

paso_actual

datos_parciales

timestamp_ultima_interaccion

Esto permite:

✅ Saber en qué menú está el usuario
✅ Controlar flujos paso a paso
✅ Mantener estado entre mensajes

5️⃣ Módulo Agenda (Citas)

Cuando el usuario selecciona Agenda, el bot muestra:

1. Agendar una nueva cita  
2. Consultar tu agenda  
3. Reprogramar una cita  
4. Cancelar una cita  
9. Volver al menú principal  


El flujo:

Actualiza pantalla_actual = agenda

Maneja pasos con paso_actual

Valida datos como fechas (YYYY-MM-DD)

Controla errores de formato

6️⃣ Validaciones

Se usan nodos IF para:

Validar formato de fecha con regex

Verificar si hay paso activo

Detectar datos inválidos

Ejemplo:

Formato requerido: YYYY-MM-DD  
Ejemplo: 2026-01-28

7️⃣ Manejo de errores

Si el usuario escribe algo inválido:

El bot responde con un mensaje amigable

Solicita nuevamente el dato correcto

Evita que el flujo se rompa

📊 Google Sheets como base de datos

La hoja sessions se usa como:

Control de estado del usuario

Simulación de sesiones

Persistencia de contexto

Cada interacción actualiza la fila correspondiente.

🚀 Flujo general

Usuario escribe al bot en Telegram

Telegram Trigger recibe el mensaje

Se consulta Google Sheets

Se evalúa pantalla y paso actual

Switch decide qué acción ejecutar

Se responde al usuario

Se actualiza estado en Google Sheets


🛠 Requisitos para correr el proyecto

Cuenta en n8n

Bot de Telegram creado con BotFather

Credenciales de Google Sheets en n8n

ngrok configurado para webhooks

Variable de entorno WEBHOOK_URL
