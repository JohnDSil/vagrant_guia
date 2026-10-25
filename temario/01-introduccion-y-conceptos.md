# 01. Introducción y Arquitectura del Entorno

## 1.1 Objetivo
Aprender el procedimiento para generar un Vagrant Box personalizado basado en **Debian 13 (Trixie)**, preparado para utilizarse como plantilla base al desplegar máquinas virtuales mediante Vagrant con el hipervisor **KVM/QEMU** y el gestor **libvirt**.

## 1.2 ¿Qué es Vagrant?
Vagrant es una herramienta que automatiza la definición, despliegue y administración de entornos de máquinas virtuales mediante archivos de configuración denominados `Vagrantfile`.

En lugar de instalar y configurar cada máquina virtual de forma manual, se definen sus parámetros (memoria, CPU, red, aprovisionamiento) en el `Vagrantfile`. Vagrant utiliza una imagen base (box) para crear la máquina y ejecutar tareas automáticas.

## 1.3 Estructura de un Vagrant Box
Un Vagrant Box es un paquete comprimido (generalmente en formato `.box` o tar) que contiene la imagen de la máquina virtual y los metadatos necesarios para que un proveedor determinado pueda utilizarla.

Para el proveedor **libvirt**, el paquete debe contener:
- `box.img`: El disco virtual de la máquina, comúnmente en formato QCOW2.
- `metadata.json`: Metadatos que definen el proveedor (`libvirt`), el formato y el tamaño virtual de la imagen.
- `Vagrantfile`: Configuración predeterminada básica que se incorpora al interior de la plantilla.

> **Diferencia clave:** El **box** es la plantilla base de solo lectura; cada vez que se ejecuta `vagrant up`, se crea una instancia independiente a partir de dicha plantilla.

*Nota sobre registros públicos:* HCP Vagrant Registry dejó de admitir la creación de nuevos boxes el 1 de octubre de 2026, con finalización prevista para el 31 de diciembre de 2026. La creación, instalación y uso de boxes a nivel local no depende de ese registro.

## 1.4 Flujo de Arquitectura
1. Vagrant lee las directivas del `Vagrantfile`.
2. Localiza el box indicado en la configuración.
3. El proveedor `libvirt` crea la máquina virtual a partir de la imagen del box.
4. La máquina arranca acelerada por hardware sobre KVM/QEMU.
5. Vagrant se conecta a la máquina a través de SSH para controlarla.
6. Se aplican las tareas de aprovisionamiento si han sido configuradas.
