# 16. Conclusiones y Referencias Oficiales

## 16.1 Conclusiones Técnicas
- Un Vagrant Box no es una imagen de disco virtual aislada: es un archivo tar empaquetado que contiene el disco (`box.img`), el descriptor de hipervisor (`metadata.json`) y el archivo de arranque básico (`Vagrantfile`).
- El éxito de la plantilla radica en la correcta configuración interna de la máquina base: usuario `vagrant` con `NOPASSWD` en sudoers, OpenSSH activo con la clave pública adecuada y la limpieza exhaustiva de historiales y archivos temporales antes del apagado.
- La distribución local no depende de registros remotos centralizados, permitiendo crear laboratorios desechables, portables y reproducibles con KVM y libvirt.
- Para despliegues a gran escala, la evolución lógica consiste en delegar el proceso manual en herramientas de automatización como Packer.

## 16.2 Referencias
1. HashiCorp - Creating a Base Box: `https://developer.hashicorp.com/vagrant/docs/boxes/base`
2. HashiCorp - Box File Format: `https://developer.hashicorp.com/vagrant/docs/boxes/format`
3. HashiCorp - Packer QEMU Plugin: `https://developer.hashicorp.com/packer/integrations/hashicorp/qemu`
4. Vagrant Libvirt Documentation: `https://vagrant-libvirt.github.io/vagrant-libvirt/`
5. José Domingo - Creación de un box para Vagrant: `https://www.josedomingo.org/pledin/2025/09/creacion-box-vagrant/`
