# 01. Introducción y Conceptos Básicos

## 1.1 Objetivo
El propósito de esta guía es aprender a construir un **Vagrant Box** personalizado basado en **Debian 13 (Trixie)**[cite: 249]. Esta plantilla servirá como base para desplegar rápidamente nuevas máquinas virtuales mediante Vagrant[cite: 249].

A lo largo del procedimiento se aprende a:
- Conocer la estructura interna de un archivo `.box`[cite: 249].
- Preparar y actualizar un sistema operativo Debian 13[cite: 249].
- Configurar el acceso administrativo por SSH requerido por Vagrant[cite: 249].
- Realizar labores de saneamiento, depuración y optimización de espacio en el disco virtual[cite: 249].
- Convertir y comprimir imágenes en formato QCOW2[cite: 249, 250].
- Empaquetar e incorporar el box al catálogo local[cite: 249].
- Validar el box creando instancias de prueba[cite: 249].

> **Entorno de destino:** Esta guía utiliza **KVM/QEMU** y **libvirt** como proveedor de virtualización[cite: 249]. Los comandos y configuraciones resultantes están diseñados para este stack y no pueden utilizarse directamente en VirtualBox sin adaptaciones[cite: 249].

## 1.2 ¿Qué es Vagrant?
Vagrant es una herramienta de automatización diseñada para definir, arrancar y administrar entornos reproducibles de máquinas virtuales a partir de archivos de configuración declarativos denominados `Vagrantfile`[cite: 249, 250].

En lugar de instalar y configurar manualmente cada sistema operativo, el `Vagrantfile` describe el hardware virtual (memoria RAM, cantidad de vCPUs, interfaces de red) y ejecuta tareas de aprovisionamiento de software automáticamente[cite: 250].

## 1.3 ¿Qué es un Vagrant Box?
Un Vagrant Box es un paquete comprimido que contiene la imagen del disco virtual junto con los metadatos necesarios para que un proveedor concreto pueda gestionarla[cite: 250].

Para el proveedor **libvirt**, el box contiene:
- `box.img`: Imagen del disco virtual de la máquina, comúnmente en formato **QCOW2**[cite: 250].
- `metadata.json`: Fichero JSON que indica el proveedor (`libvirt`) y las dimensiones virtuales de la imagen[cite: 250, 260].
- `Vagrantfile`: Configuración básica por defecto incorporada en la propia plantilla[cite: 250].

*Diferenciación conceptual:* El **box** actúa como la plantilla estática de partida; cada vez que se lanza `vagrant up`, se crea una instancia virtual independiente a partir de dicha plantilla[cite: 250].

*Aviso sobre registros en la nube:* En octubre de 2026, el servicio público HCP Vagrant Registry dejó de admitir la creación de nuevos boxes y su retirada definitiva está programada para el 31 de diciembre de 2026[cite: 250, 265]. La creación, gestión e instalación de archivos `.box` en local opera de forma completamente autónoma sin depender de servicios externos[cite: 250].

## 1.4 Arquitectura del Entorno
El flujo operativo se estructura en las siguientes etapas[cite: 250]:
1. Vagrant analiza el archivo `Vagrantfile` del proyecto[cite: 250].
2. Localiza e identifica el box especificado en el registro local o remoto[cite: 250].
3. El proveedor `libvirt` instancia la máquina virtual empleando la imagen del box[cite: 250].
4. La máquina virtual se ejecuta acelerada por hardware sobre KVM/QEMU[cite: 250].
5. Vagrant establece conexión con el huésped mediante SSH[cite: 250].
6. Se aplican las recetas de aprovisionamiento configuradas[cite: 250].
