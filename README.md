# Proyecto Honeypot

Una implementación sencilla de honeypot para capturar y monitorear actividad maliciosa. Este proyecto utiliza Flask para crear una aplicación web vulnerable y configura un servicio SSH con el que los atacantes pueden interactuar. Incluye scripts de registro y monitorización para rastrear y analizar la actividad.

## Índice

- [Características](#características)
- [Instalación](#instalación)
- [Configuración](#configuración)
- [Uso](#uso)
- [Monitorización](#monitorización)
- [Notas](#notas)
- [Licencia](#licencia)

## Características

- Aplicación web vulnerable basada en Flask
- Servicio SSH configurado con credenciales débiles
- Registro de comandos ejecutados a través de la aplicación web
- Monitorización en tiempo real de los registros del honeypot
- Próximamente más funciones...⌛

0. **Preconfiguración**

Crea un nuevo usuario en tu sistema para crear Esa cuenta se llama Honeypot. sudo useradd -m -s /bin/bash usuario_vulnerable # Cambia el usuario_vulnerable por el nombre de usuario que desees
sudo passwd usuario_vulnerable # Establece una contraseña débil como 'password123', 'admin' o 'root'

## Instalación

1. **Clonar el repositorio:**

```bash
git clone https://github.com/whxitte/Honeypot.git
cd Honeypot
```

2. **Crear y activar un entorno virtual de Python:**

```bash
python -m venv honeypot-env
source honeypot-env/bin/activate # Para Windows, usa `honeypot-env\Scripts\activate`
```

3. **Instalar los paquetes de Python necesarios:**

```bash
pip install -r requirements.txt
```

4. **Instalar y configurar SSH:**

```bash
sudo apt-get install openssh-server
sudo nano /etc/ssh/sshd_config
```

Edite el archivo de configuración de SSH (`/etc/ssh/sshd_config`) para permitir la autenticación con contraseña. Agregue o modifique las siguientes líneas:

```
PermitRootLogin yes
PasswordAuthentication yes
PermitEmptyPasswords yes # Opcional, pero aumenta la vulnerabilidad
```

Reinicie el servicio SSH:

```bash
sudo systemctl restart ssh
```

## Configuración

1. **Ejecute la aplicación Flask y el servicio SSH:**

```bash
sudo su
./run_honeypot.sh
```

2. **Monitoree los registros en tiempo real:**

```bash
>> tail -f /var/log/auth.log # Para registros SSH
o
>> sudo journalctl -u ssh -f (si el comando anterior para SSH no funciona)
o verifique el registro SSH en su sistema / monitorícelo en tiempo real

>> tail -f /var/log/honeypot.log # Para registros de la aplicación Flask
```

## Uso

- Acceda a la aplicación web vulnerable en [http://localhost](http://localhost)
- Utilice el endpoint `/vulnerable` para ejecutar comandos. Por ejemplo:

```bash
http://localhost/vulnerable?cmd=ls
```

- La salida de los comandos y cualquier error se registrarán en `/var/log/honeypot.log`.

## Monitoreo

Para monitorear la actividad del honeypot, puede usar el script `monitor_honeypot.py`:

```bash
python monitor_honeypot.py
```

Este script imprimirá las nuevas entradas del registro en una tabla formateada en tiempo real.

## Notas

- Asegúrese de ajustar los permisos y la configuración según sus necesidades de seguridad.
- Esta configuración es vulnerable intencionalmente con fines educativos y no debe utilizarse en un entorno de producción.