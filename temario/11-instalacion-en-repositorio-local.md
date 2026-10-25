# 11. Instalación del Box en el Repositorio Local de Vagrant

Para utilizar el nuevo box no es necesario publicarlo en internet; se instala directamente en el catálogo local de Vagrant.

## 11.1 Registro del Box
Desde el directorio de trabajo, registrar la plantilla en el entorno del usuario:
```bash
cd ~/vagrant-box-debian13
vagrant box add \
  --name debian13-libvirt \
  --provider libvirt \
  debian13-libvirt.box
```

- `--name debian13-libvirt`: Identificador con el que se invocará la plantilla en los proyectos.
- `--provider libvirt`: Indica que el box está empaquetado para KVM/libvirt.

## 11.2 Verificación del Catálogo
Listar los boxes disponibles en el sistema:
```bash
vagrant box list
```
*Debe mostrarse en la salida: `debian13-libvirt (libvirt, 0)`.*

> **Nota:** Este registro es local al usuario que ejecuta el comando. Otros usuarios del mismo anfitrión requerirán añadir el box a sus respectivos repositorios locales.
