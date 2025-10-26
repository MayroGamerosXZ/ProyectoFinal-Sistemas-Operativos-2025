
# Guía Técnica: Comunicación entre VM de Windows y VM de Ubuntu

## Objetivo
Establecer conexión SSH y transferencia de archivos entre dos máquinas virtuales (Windows y Ubuntu) en VirtualBox.

---

## 🔧 Requisitos Previos
- Ambas VMs deben usar el modo "Adaptador puente" en VirtualBox (misma red física).
- Tener instalado OpenSSH Server en Ubuntu.
- Tener cliente SSH en Windows (integrado desde Windows 10 versión 1809 en adelante).
- Ambas VMs deben tener direcciones IP en la misma subred (ej. `192.168.1.x`).

---

## 1. Identificar Usuarios

### En Windows (PowerShell)
```powershell
$env:USERNAME
whoami
```

### En Ubuntu (Terminal)
```bash
whoami
echo $USER
```

✅ Ejemplo de usuario en Ubuntu: `mayrogameros`
✅ Ejemplo de usuario en Windows: `Mayro`

---

## 2. Obtener Direcciones IP

### En Windows (CMD)
```cmd
ipconfig
```
Buscar: `IPv4 Address . . . . . . . . . . . : 192.168.1.41`

### En Ubuntu (Terminal)
```bash
ip a
```
Buscar en `enp0s3`: `inet 192.168.1.42/24`

⚠️ Ignorar direcciones como `127.0.0.1`, `10.0.2.15` (NAT) o IPv6 link-local.

---

## 3. Configurar SSH en Ubuntu

### Instalar y activar el servidor SSH
```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

### (Opcional) Permitir puerto 22 en el firewall
```bash
sudo ufw allow 22
```

✅ El servicio debe estar activo y escuchando en todas las interfaces (`0.0.0.0:22`).

---

## 4. Conexión SSH desde Windows a Ubuntu

### Ejecutar en PowerShell (Windows)
```powershell
ssh mayrogameros@192.168.1.42
```
Reemplazar `mayrogameros` por el usuario real de Ubuntu.
Reemplazar `192.168.1.42` por la IP real de Ubuntu.
Ingresar la contraseña del usuario en Ubuntu.

✅ Una vez conectado, todos los comandos se ejecutan en Ubuntu.
Salir con:
```bash
exit
```

---

## 5. Transferencia de Archivos con SCP

### 5.1 Desde Windows → Ubuntu
```powershell
scp "C:\Users\Mayro\Downloads\Hola_a_Todos.txt" mayrogameros@192.168.1.42:/home/mayrogameros/
```
✅ Usar comillas si hay espacios en la ruta de Windows.

### 5.2 Desde Ubuntu → Windows
```powershell
scp mayrogameros@192.168.1.42:/home/mayrogameros/archivo1.txt "C:\Archivos_de_Conexion\"
```
⚠️ La carpeta de destino en Windows debe existir. Para crearla:
```powershell
mkdir "C:\Archivos_de_Conexion"
```

---

## 6. Verificación de Conectividad

### Desde Windows
```powershell
ping 192.168.1.42
```

### Desde Ubuntu
```bash
ping 192.168.1.41
```

✅ Si el ping falla, revisar configuración de red en VirtualBox (ambas VMs deben estar en Adaptador puente).

---

## 🛠️ Solución de Errores Comunes

### `Connection refused`
- SSH no activo en Ubuntu o IP incorrecta.
- Verificar con:
```bash
sudo systemctl status ssh
```
- Usar IP de red real (`192.168.1.x`).

### `Could not resolve hostname`
- Ejecutar `scp` desde la máquina destino.
- Para copiar a Windows, ejecutar desde PowerShell.

### `Permission denied`
- Contraseña incorrecta o usuario inexistente.
- Verificar usuario con:
```bash
whoami
```

---

## ✅ Logros Verificados
- Ambas VMs en misma red (`192.168.1.x`) mediante Adaptador puente.
- Servidor SSH activo en Ubuntu.
- Conexión SSH exitosa desde Windows a Ubuntu.
- Transferencia de archivos en ambos sentidos con `scp`.
- Usuarios y rutas correctamente identificados.

---

## 💡 Sugerencias Finales
- Usa nombres de archivo sin espacios para evitar problemas con `scp`.
- Si usas frecuentemente `scp`, considera configurar claves SSH para evitar ingresar la contraseña cada vez.
- Para automatizar copias, crea scripts en PowerShell o Bash con los comandos `scp` ya listos.

📝 Esta guía excluye el uso de carpetas compartidas de VirtualBox, según solicitud.

Basado en entorno: VirtualBox + Windows 10/11 VM + Ubuntu 22.04 VM.

---

Fin del documento.
