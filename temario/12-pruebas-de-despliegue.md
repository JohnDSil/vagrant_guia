# 12. Pruebas de Despliegue y Validación de la Plantilla

La validación definitiva consiste en crear una máquina virtual totalmente nueva a partir del box recién registrado.

## 12.1 Creación de un Proyecto de Prueba
Crear un directorio independiente en el sistema anfitrión:
```bash
mkdir -p ~/prueba-debian13
cd ~/prueba-debian13
```

Crear el `Vagrantfile` de prueba:
```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "debian13-libvirt"
  config.vm.hostname = "debian13-prueba"
  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.memory = 2048
    libvirt.cpus = 2
  end
end
```

## 12.2 Despliegue y Arranque
Arrancar la instancia especificando el proveedor libvirt:
```bash
vagrant up --provider=libvirt
```

Comprobar que la máquina está operativa:
```bash
vagrant status
```

## 12.3 Comprobaciones Internas vía SSH
Acceder a la máquina virtual:
```bash
vagrant ssh
```

Ejecutar las comprobaciones de verificación del entorno:
- Comprobar la versión del sistema operativo:
  ```bash
  cat /etc/os-release
  ```
- Comprobar el usuario de ejecución:
  ```bash
  whoami
  ```
- Verificar el direccionamiento IP y la tabla de enrutamiento:
  ```bash
  ip addr
  ip route
  ```
- Verificar la resolución de nombres DNS:
  ```bash
  getent hosts deb.debian.org
  ```

Finalizadas las comprobaciones, salir de la máquina:
```bash
exit
```

## 12.4 Parada y Destrucción de la Instancia de Prueba
Detener la máquina de pruebas:
```bash
vagrant halt
```

Eliminar la instancia y liberar los recursos asociados:
```bash
vagrant destroy -f
```
*La eliminación de la máquina de prueba no elimina la plantilla del catálogo local.*
