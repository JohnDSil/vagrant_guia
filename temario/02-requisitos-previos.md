# 02. Requisitos Previos y Preparación del Sistema Anfitrión

Para desplegar este entorno es necesario disponer de un equipo con un sistema operativo Linux anfitrión que admita virtualización por hardware.

## 2.1 Comprobación de la Arquitectura y Virtualización
Comprobar que el anfitrión es una arquitectura de 64 bits (x86_64):
```bash
uname -m
```

Verificar si el procesador expone las extensiones de virtualización por hardware (`vmx` en procesadores Intel o `svm` en AMD):
```bash
grep -Eoc '(vmx|svm)' /proc/cpuinfo
```
*Si devuelve un número mayor que 0, el procesador detecta las extensiones. Si devuelve 0, deben activarse en la BIOS/UEFI de la máquina o habilitar la virtualización anidada si el host es a su vez una máquina virtual.*

## 2.2 Instalación de Herramientas de Virtualización
En Debian o Ubuntu, instalar el paquete de herramientas de QEMU y libvirt:
```bash
sudo apt update
sudo apt install -y qemu-system-x86 qemu-utils libvirt-daemon-system libvirt-clients virtinst cpu-checker
```

Verificar que la aceleración KVM está disponible:
```bash
sudo kvm-ok
# O comprobar que los módulos del kernel están cargados:
lsmod | grep kvm
```

Habilitar y poner en marcha el demonio de libvirt:
```bash
sudo systemctl enable --now libvirtd
sudo systemctl status libvirtd
```

## 2.3 Instalación de Vagrant
Instalar Vagrant siguiendo el procedimiento oficial de la distribución y comprobar la versión instalada:
```bash
vagrant --version
```

## 2.4 Instalación del Proveedor `vagrant-libvirt`
Instalar el plugin necesario para conectar Vagrant con libvirt:
```bash
vagrant plugin install vagrant-libvirt
```

Comprobar que el complemento se encuentra instalado:
```bash
vagrant plugin list
```
*(Debe listar `vagrant-libvirt`. Si existieran fallos de compilación nativa, se deben instalar las cabeceras y librerías de desarrollo de libvirt y Ruby correspondientes a la distribución).*

## 2.5 Comprobación de la Conexión con Libvirt
Comprobar el acceso a las instancias administradas por libvirt a nivel de sistema:
```bash
virsh -c qemu:///system list --all
```
*Si se producen errores de permisos, deben configurarse los grupos locales del usuario (como `libvirt` o `kvm`). No se recomienda ejecutar permanentemente Vagrant bajo privilegios de administrador `sudo` para evitar problemas de propiedad de ficheros.*
