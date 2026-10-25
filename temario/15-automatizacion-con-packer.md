# 15. Alternativa para Automatización del Proceso: Packer

El procedimiento manual permite comprender la arquitectura de un box. Sin embargo, si se requiere generar plantillas de forma periódica, controlada y reproducible, la herramienta recomendada es **Packer**.

## 15.1 ¿Qué aporta Packer?
Packer es una herramienta de infraestructura como código que permite automatizar la creación de imágenes para múltiples plataformas a partir de un único archivo de configuración.

- Automatiza la instalación del sistema operativo sin intervención interactiva.
- Aplica configuraciones, usuarios y claves SSH mediante scripts.
- Construye imágenes para KVM mediante el complemento oficial **QEMU de Packer**.
- Facilita versionar los cambios de la plantilla en repositorios Git y reconstruir el box periódicamente.

## 15.2 Recursos Oficiales de Packer
- Documentación oficial de Packer: [developer.hashicorp.com/packer](https://developer.hashicorp.com/packer)
- Plugin QEMU para Packer: [developer.hashicorp.com/packer/integrations/hashicorp/qemu](https://developer.hashicorp.com/packer/integrations/hashicorp/qemu)
- Especificación oficial del formato de un Box: [developer.hashicorp.com/vagrant/docs/boxes/format](https://developer.hashicorp.com/vagrant/docs/boxes/format)
