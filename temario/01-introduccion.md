# 01. Introducción y Conceptos Básicos

## 1.1 Objetivo
El propósito de esta guía es aprender a construir un **Vagrant Box** personalizado basado en **Debian 13 (Trixie)**. Esta plantilla servirá como base para desplegar rápidamente nuevas máquinas virtuales mediante Vagrant.

A lo largo del procedimiento se aprende a:
- Conocer la estructura interna de un archivo `.box`.
- Preparar y actualizar un sistema operativo Debian 13.
- Configurar el acceso administrativo por SSH requerido por Vagrant.
- Realizar labores de saneamiento, depuración y optimización de espacio en el disco virtual.
- Convertir y comprimir imágenes en formato QCOW2.
- Empaquetar e incorporar el box al catálogo local.
- Validar el box creando instancias de prueba.

> **Entorno de destino:** Esta guía utiliza **KVM/QEMU** y **libvirt** como proveedor de virtualización. Los comandos y configuraciones resultantes están diseñados para este stack y no pueden utilizarse directamente en VirtualBox sin adaptaciones.

## 1.2 ¿Qué es Vagrant?
Vagrant es una herramienta de automatización diseñada para definir, arrancar y administrar entornos reproducibles de máquinas virtuales a partir de archivos de configuración declarativos denominados `Vagrantfile`.

En lugar de instalar y configurar manualmente cada sistema operativo, el `Vagrantfile` describe el hardware virtual (memoria RAM, cantidad de vCPUs, interfaces de red) y ejecuta tareas de aprovisionamiento de software automáticamente.

## 1.3 ¿Qué es un Vagrant Box?
Un Vagrant Box es un paquete comprimido que contiene la imagen del disco virtual junto con los metadatos necesarios para que un proveedor concreto pueda gestionarla.

Para el proveedor **libvirt**, el box contiene:
- `box.img`: Imagen del disco virtual de la máquina, comúnmente en formato **QCOW2**.
- `metadata.json`: Fichero JSON que indica el proveedor (`libvirt`) y las dimensiones virtuales de la imagen.
- `Vagrantfile`: Configuración básica por defecto incorporada en la propia plantilla.

*Diferenciación conceptual:* El **box** actúa como la plantilla estática de partida; cada vez que se lanza `vagrant up`, se crea una instancia virtual independiente a partir de dicha plantilla.

*Aviso sobre registros en la nube:* En octubre de 2026, el servicio público HCP Vagrant Registry dejó de admitir la creación de nuevos boxes y su retirada definitiva está programada para el 31 de diciembre de 2026. La creación, gestión e instalación de archivos `.box` en local opera de forma completamente autónoma sin depender de servicios externos.

## 1.4 Arquitectura del Entorno
El flujo operativo se estructura en las siguientes etapas:
1. Vagrant analiza el archivo `Vagrantfile` del proyecto.
2. Localiza e identifica el box especificado en el registro local o remoto.
3. El proveedor `libvirt` instancia la máquina virtual empleando la imagen del box.
4. La máquina virtual se ejecuta acelerada por hardware sobre KVM/QEMU.
5. Vagrant establece conexión con el huésped mediante SSH.
6. Se aplican las recetas de aprovisionamiento configuradas.
