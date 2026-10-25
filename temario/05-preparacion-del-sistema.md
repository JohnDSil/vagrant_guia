# 05. Preparación de Debian para Convertirlo en un Box

La máquina invitada debe disponer de herramientas de red, un usuario específico de gestión y los permisos SSH requeridos por Vagrant.

## 5.1 Instalación de Herramientas Básicas
Dentro de la máquina base, asegurar la presencia de los paquetes requeridos:
```bash
sudo apt update
sudo apt install -y openssh-server sudo curl ca-certificates
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

## 5.2 Comprobación y Creación del Usuario `vagrant`
La máquina debe contar obligatoriamente con el usuario `vagrant`. Comprobar su existencia:
```bash
id vagrant
```
Si no existe, se crea y se añade al grupo administrativo `sudo`:
```bash
sudo adduser vagrant
sudo usermod -aG sudo vagrant
```

## 5.3 Configuración de `sudo` sin Contraseña
Para que los scripts de aprovisionamiento de Vagrant operen de forma desatendida, el usuario `vagrant` debe poder ejecutar `sudo` sin solicitud de clave.

Crear el archivo en `sudoers.d`:
```bash
sudo visudo -f /etc/sudoers.d/vagrant
```

Añadir la directiva:
```text
vagrant ALL=(ALL) NOPASSWD: ALL
```

Comprobar la sintaxis y fijar los permisos adecuados:
```bash
sudo visudo -cf /etc/sudoers.d/vagrant
sudo chmod 0440 /etc/sudoers.d/vagrant
```

## 5.4 Configuración de la Clave SSH Insegura de Vagrant
Vagrant utiliza un par de claves públicas estandarizadas (*insecure public key*) para el primer acceso a entornos de laboratorio.

1. Descargar la clave pública oficial de Vagrant:
```bash
curl -fsSL [https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub](https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub) -o /tmp/vagrant.pub
test -s /tmp/vagrant.pub && echo "Clave descargada correctamente"
```

2. Instalar la clave pública en el directorio `.ssh` del usuario `vagrant` con los permisos estrictos de OpenSSH:
```bash
sudo install -d -m 0700 -o vagrant -g vagrant /home/vagrant/.ssh
sudo install -m 0600 -o vagrant -g vagrant /tmp/vagrant.pub /home/vagrant/.ssh/authorized_keys
rm -f /tmp/vagrant.pub
```

> **Advertencia de seguridad:** Esta clave es de dominio público. Es adecuada para laboratorios de prácticas, pero para entornos de producción o redes expuestas debe sustituirse por mecanismos de autenticación seguros y privados.

## 5.5 Comprobación del Servicio OpenSSH
Verificar la sintaxis del daemon SSH y reiniciar el servicio:
```bash
sudo sshd -t
sudo systemctl restart ssh
```
*No es necesario habilitar acceso mediante contraseña; la autenticación por clave pública es suficiente.*
