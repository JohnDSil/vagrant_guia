# 03. Preparación de la Máquina Base

Existen dos vías para generar la máquina base: realizar una instalación limpia desde una ISO de Debian 13 o utilizar un box previo compatible con libvirt (por ejemplo, Debian 12) y actualizarlo. En esta guía se sigue la segunda vía para aprovechar una base ya adaptada a Vagrant.

> **Importante:** La máquina de partida debe ser compatible con libvirt. No sirven imágenes preparadas exclusivamente para VirtualBox sin adaptar.

## 3.1 Crear el Directorio de Trabajo
Crear una carpeta específica en el anfitrión:
```bash
mkdir -p ~/vagrant-box-debian13
cd ~/vagrant-box-debian13
```

## 3.2 Crear el `Vagrantfile` de la Máquina Base
Crear un archivo `Vagrantfile` para arrancar la máquina de partida:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "debian12-libvirt"
  config.vm.hostname = "debian13-base"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.memory = 2048
    libvirt.cpus = 2
  end
end
```

- Se asigna el nombre de equipo `debian13-base`.
- Se asignan 2048 MB de RAM y 2 vCPUs.
- Se **deshabilita la carpeta compartida** (`synced_folder disabled: true`) para evitar que la plantilla dependa del directorio local del anfitrión durante la preparación.

## 3.3 Arrancar y Conectar a la Máquina Base
Desplegar la máquina sobre libvirt:
```bash
vagrant up --provider=libvirt
```

Acceder por SSH al interior del sistema:
```bash
vagrant ssh
```

Inspeccionar la versión del sistema operativo en ejecución:
```bash
cat /etc/os-release
```
*Si la máquina ya reporta Debian 13, se continúa directamente al paso de configuración. Si reporta Debian 12, se ejecuta el proceso de actualización.*
