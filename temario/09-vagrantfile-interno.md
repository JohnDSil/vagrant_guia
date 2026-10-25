# 09. Configuración del Vagrantfile Interno del Box

El paquete puede incluir un archivo `Vagrantfile` interno que establece los parámetros por defecto de la máquina virtual cuando los proyectos no los sobreescriban expresamente.

## 9.1 Creación del Fichero
Dentro del directorio `~/vagrant-box-debian13/box/`:
```bash
nano Vagrantfile
```

Añadir la configuración básica recomendada:
```ruby
Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.memory = 1024
    libvirt.cpus = 1
  end
end
```

## 9.2 Propósito de estas Directivas
- `synced_folder disabled: true`: Desactiva el montaje automático de carpetas compartidas para no exigir dependencias ni herramientas adicionales al arrancar.
- `libvirt.driver = "kvm"`: Asegura la aceleración por hardware de KVM.
- `libvirt.memory = 1024` y `libvirt.cpus = 1`: Asigna valores iniciales moderados (1 GB de RAM y 1 CPU), los cuales podrán ampliarse en el `Vagrantfile` de cada proyecto particular.
