# Creación de un Vagrant Box Personalizado con Debian 13, KVM y Libvirt

Repositorio con la guía completa y estructurada para construir, configurar, empaquetar, probar y distribuir una plantilla (Vagrant Box) personalizada en sistemas Linux utilizando virtualización basada en KVM/QEMU y libvirt.

---

## 📑 Índice de Contenidos

| Nº | Tema | Archivo | Resumen de Contenidos |
| :---: | :--- | :--- | :--- |
| **01** | Introducción y Arquitectura | [`01-introduccion-y-conceptos.md`](temario/01-introduccion-y-conceptos.md) | Qué es Vagrant, anatomía de un Box (`box.img`, `metadata.json`, `Vagrantfile`) y arquitectura KVM/libvirt. |
| **02** | Requisitos y Preparación del Host | [`02-requisitos-y-entorno.md`](temario/02-requisitos-y-entorno.md) | Comprobación de CPU (`vmx`/`svm`), instalación de QEMU, libvirt, Vagrant y plugin `vagrant-libvirt`. |
| **03** | Máquina Base y Actualización | [`03-maquina-base-y-actualizacion.md`](temario/03-maquina-base-y-actualizacion.md) | Despliegue de la base inicial y procedimiento de actualización a Debian 13 (Trixie). |
| **04** | Configuración Interna del Box | [`04-configuracion-acceso-y-ssh.md`](temario/04-configuracion-acceso-y-ssh.md) | Usuario `vagrant`, sudo sin contraseña (`NOPASSWD`) y clave pública insegura de Vagrant. |
| **05** | Limpieza y Optimización | [`05-limpieza-y-apagado.md`](temario/05-limpieza-y-apagado.md) | Purga de APT, borrado de archivos temporales, vaciado de historiales bash y comando `fstrim`. |
| **06** | Conversión de Disco y Metadatos | [`06-conversion-disco-y-metadatos.md`](temario/06-conversion-disco-y-metadatos.md) | Copia de imagen, compresión QCOW2 con `qemu-img`, redacción de `metadata.json` y `Vagrantfile` interno. |
| **07** | Empaquetado e Instalación Local | [`07-empaquetado-e-instalacion.md`](temario/07-empaquetado-e-instalacion.md) | Creación del `.box` con `tar`, cálculo de hash SHA-256 e instalación con `vagrant box add`. |
| **08** | Pruebas de Despliegue y Validación | [`08-pruebas-despliegue.md`](temario/08-pruebas-despliegue.md) | Despliegue de una máquina nueva desde el box generado, validación SSH/red y limpieza con `vagrant destroy`. |
| **09** | Distribución, Errores y Automatización | [`09-distribucion-troubleshooting-packer.md`](temario/09-distribucion-troubleshooting-packer.md) | Métodos de entrega, resolución de problemas típicos y automatización avanzada con Packer. |

---

## 🛠️ Especificaciones Técnicas del Entorno
- **Sistema Anfitrión:** Linux con virtualización por hardware (Intel VT-x o AMD-V).
- **Virtualización:** KVM / QEMU.
- **Gestor:** libvirt con el complemento `vagrant-libvirt`.
- **Sistema Invitado de la Plantilla:** Debian 13 (Trixie).
