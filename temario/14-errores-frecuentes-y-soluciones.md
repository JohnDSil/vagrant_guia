# 14. Errores Frecuentes y Procedimientos de Diagnóstico

## 14.1 No se encuentra el proveedor libvirt
- Comprobar que el plugin está cargado:
  ```bash
  vagrant plugin list
  ```
- Si no aparece, reinstalar con `vagrant plugin install vagrant-libvirt`.
- Si falla por dependencias nativas de C o Ruby, revisar los paquetes de desarrollo de libvirt de la distribución.

## 14.2 Errores de permisos al interactuar con libvirt
- Comprobar la conexión ejecutando `virsh -c qemu:///system list --all`.
- Comprobar la pertenencia del usuario a los grupos `libvirt` y `kvm`.
- **No solucionar el problema ejecutando Vagrant con `sudo`**, ya que crea artefactos asignados a root y altera las rutas del repositorio local de boxes.

## 14.3 Fallo al registrar el box con `vagrant box add`
- Comprobar que el archivo es un archivo tar válido y contiene los 3 ficheros obligatorios:
  ```bash
  tar -tzf debian13-libvirt.box
  ```
- Validar la sintaxis de `metadata.json` con `python3 -m json.tool metadata.json`.
- Confirmar que el campo `"provider"` coincide con el proveedor utilizado.

## 14.4 La máquina arranca pero falla la autenticación SSH
Verificar dentro de la máquina base:
1. Que el servicio OpenSSH esté activo (`systemctl status ssh`).
2. Que el usuario `vagrant` exista.
3. Que el archivo `/home/vagrant/.ssh/authorized_keys` contenga la clave pública oficial.
4. Que los permisos sean estrictos: `0700` para `.ssh` y `0600` para `authorized_keys`.

## 14.5 La máquina invitada carece de acceso a Internet
- Comprobar la asignación de dirección IP y puerta de enlace dentro del huésped con `ip addr` e `ip route`.
- Verificar si la red virtual predeterminada de libvirt está activa en el anfitrión y entregando direcciones mediante DHCP.
- Verificar la configuración de servidores DNS en `/etc/resolv.conf`.

## 14.6 El archivo `.box` ocupa un tamaño excesivo
- Inspeccionar el tamaño real frente a la capacidad virtual:
  ```bash
  ls -lh box.img
  qemu-img info box.img
  ```
- Comprobar que se ha vaciado la caché de APT (`sudo apt clean`), eliminado archivos de `/tmp` y ejecutado `sudo fstrim -av` antes de apagar la máquina base.
- Repetir la conversión con compresión `qemu-img convert -c -O qcow2`.
