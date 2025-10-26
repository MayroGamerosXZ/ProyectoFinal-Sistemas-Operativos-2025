
# Envío de Correos desde la Terminal de Windows usando WSL (Ubuntu) y SSMTP

## ✅ Objetivo
Enviar correos electrónicos desde la terminal de Windows utilizando WSL con Ubuntu, mediante la herramienta `ssmtp`.

---

## 🔧 Requisitos Previos
- Tener instalado **WSL con Ubuntu** en Windows.
- Tener una cuenta de **Gmail**.
- Habilitar el acceso de aplicaciones menos seguras en Gmail **o** generar una **contraseña de aplicación** si tienes la verificación en dos pasos (2FA):
  - Accede a: [Contraseñas de aplicación en Google](https://myaccount.google.com/apppasswords)
  - Genera una contraseña para “Correo” → “Otro (personalizado)” → por ejemplo: “WSL”.
  - Guarda la contraseña de 16 caracteres generada.

---

## 🛠️ Paso 1: Instalar SSMTP y Mailutils

### En Ubuntu (WSL)
```bash
sudo apt update
sudo apt install ssmtp mailutils -y
```

---

## 🛠️ Paso 2: Configurar SSMTP

### Editar el archivo de configuración
```bash
sudo nano /etc/ssmtp/ssmtp.conf
```

### Contenido del archivo (reemplaza con tus datos):
```ini
root=tu_correo@gmail.com
mailhub=smtp.gmail.com:587
AuthUser=tu_correo@gmail.com
AuthPass=tu_contraseña_de_aplicacion
UseSTARTTLS=YES
FromLineOverride=YES
```

⚠️ **Importante:** La contraseña debe ser la contraseña de aplicación de 16 caracteres, **no** tu contraseña habitual de Gmail.

Guarda y cierra el archivo con `Ctrl+O`, `Enter`, `Ctrl+X`.

---

## 🧪 Paso 3: Probar el Envío de Correo

### Enviar mensaje simple
```bash
echo "Este es el cuerpo del mensaje" | mail -s "Asunto de prueba" destinatario@example.com
```

### Ejemplo: enviarte un correo a ti mismo
```bash
echo "Hola desde WSL!" | mail -s "Prueba desde Ubuntu en WSL" tu_correo@gmail.com
```

---

## 🔍 Notas Adicionales
- Si usas una cuenta de **Google Workspace (Gmail empresarial)**, asegúrate de que el administrador permita el acceso por SMTP.
- Si el envío falla, revisa los logs:
  - `/var/log/mail.log`
  - `/var/log/mail.err`
- Alternativas a `ssmtp`:
  - `msmtp`: más moderno y flexible.
  - `curl` con la API de Gmail (requiere configuración avanzada).

---

## ✅ Recomendaciones Finales
- Usa contraseñas de aplicación para mayor seguridad.
- Evita usar tu contraseña principal de Gmail.
- Para automatizar el envío de correos, puedes crear scripts en Bash.

---

Fin del documento.
