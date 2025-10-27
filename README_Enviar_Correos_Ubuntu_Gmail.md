
# Guía Técnica: Enviar correos desde Ubuntu a Gmail (vía terminal)

## Objetivo
Configurar Ubuntu para enviar correos electrónicos a través del servidor SMTP de Gmail, usando herramientas de línea de comandos como `mail` o `ssmtp`.

---

## 🔑 Requisitos previos
- Cuenta de Gmail con verificación en dos pasos (2FA) activada.
- Contraseña de aplicación de Gmail (16 caracteres), generada en: [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
  - Seleccionar: Correo → Otro (personalizado) → Nombre: Ubuntu → Generar.
- Acceso a terminal de Ubuntu con permisos sudo.

⚠️ No uses tu contraseña normal de Gmail. Google ya no permite el acceso de "aplicaciones menos seguras".

---

## 🛠️ Paso 1: Instalar herramientas necesarias
```bash
sudo apt update
sudo apt install ssmtp mailutils -y
```
- `ssmtp`: agente de transferencia de correo simple (reenvía a SMTP externo).
- `mailutils`: proporciona el comando `mail` para enviar mensajes desde terminal.

---

## 🛠️ Paso 2: Configurar ssmtp
Editar el archivo de configuración:
```bash
sudo nano /etc/ssmtp/ssmtp.conf
```
Reemplazar todo el contenido con lo siguiente (ajustar tus datos):
```ini
root=tu_correo@gmail.com
mailhub=smtp.gmail.com:587
AuthUser=tu_correo@gmail.com
AuthPass=tu_contraseña_de_aplicacion_de_16_caracteres
UseSTARTTLS=YES
FromLineOverride=YES
```
✅ Ejemplo real:
```ini
root=mayrogameros@gmail.com
mailhub=smtp.gmail.com:587
AuthUser=mayrogameros@gmail.com
AuthPass=abcdefghijklmnop
UseSTARTTLS=YES
FromLineOverride=YES
```
→ Quitar los espacios en la contraseña.

Guardar y salir: `Ctrl+O` → `Enter` → `Ctrl+X`

---

## 🛠️ Paso 3: (Opcional) Configurar alias de correo local
Editar:
```bash
sudo nano /etc/ssmtp/revaliases
```
Agregar:
```ini
root:mayrogameros@gmail.com:smtp.gmail.com:587
mayrogameros:mayrogameros@gmail.com:smtp.gmail.com:587
```
Esto asegura que los correos enviados por el usuario `mayrogameros` usen tu Gmail como remitente.

---

## ✉️ Paso 4: Enviar un correo de prueba
### Sintaxis básica:
```bash
echo "Cuerpo del mensaje" | mail -s "Asunto" destinatario@example.com
```
### Ejemplo: enviarte a ti mismo
```bash
echo "¡Prueba exitosa desde Ubuntu!" | mail -s "Correo desde terminal" mayrogameros@gmail.com
```
### Enviar a múltiples destinatarios:
```bash
echo "Hola a todos" | mail -s "Notificación" usuario1@gmail.com usuario2@gmail.com
```
✅ El correo aparecerá en la bandeja de entrada de los destinatarios (y en "Enviados" de tu Gmail si usas la misma cuenta).

---

## 🔍 Paso 5: Verificación y solución de errores
### Ver logs de envío:
```bash
tail -f /var/log/mail.log
# o en algunos sistemas:
sudo cat /var/log/mail.err
```

---

## ✅ Recomendaciones Finales
- Usa contraseñas de aplicación para mayor seguridad.
- Evita usar tu contraseña principal de Gmail.
- Para automatizar el envío de correos, puedes crear scripts en Bash.
- Considera usar `msmtp` como alternativa moderna a `ssmtp`.

---

Fin del documento.
