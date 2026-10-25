# Guía Técnica: Creación de un Vagrant Box Personalizado (Debian 13, KVM y Libvirt)

Repositorio de documentación práctica y comandos paso a paso para la creación, optimización, empaquetado, despliegue y distribución de plantillas (boxes) personalizadas de Vagrant sobre Linux.

---

## 📑 Índice del Temario

| Nº | Sección | Archivo | Contenido Clave |
| :---: | :--- | :--- | :--- |
| **01** | Introducción y Conceptos | [`01-introduccion.md`](temario/01-introduccion.md) | Objetivo, definición de Vagrant, anatomía de un Box y arquitectura del entorno |
| **02** | Requisitos Previos | [`02-requisitos-previos.md`](temario/02-requisitos-previos.md) | Comprobación de CPU (`vmx`/`svm`), instalación de QEMU, libvirt y plugin `vagrant-libvirt` |
| **03** | Preparación de la Máquina Base | [`03-preparacion-maquina-base.md`](temario/03-preparacion-maquina-base.md) | Estructura de trabajo, definición de `Vagrantfile` inicial y arranque del entorno base |
| **04** | Actualización a Debian 13 (Trixie) | [`04-actualizacion-debian-13.md`](temario/04-actualizacion-debian-13.md) | Migración desde Bookworm: copia de seguridad de APT, reemplazo de fuentes y `full-upgrade` |
| **05** | Preparación Interna del Sistema | [`05-preparacion-del-sistema.md`](temario/05-preparacion-del-sistema.md) | Paquetes base, usuario `vagrant`, permisos en sudoers (`NOPASSWD`) y clave SSH insegura |
| **06** | Limpieza de la Máquina Base | [`06-limpieza-de-la-maquina.md`](temario/06-limpieza-de-la-maquina.md) | Purga de APT, vaciado de `/tmp`, borrado de historiales y optimización con `fstrim` |
| **07** | Localización y Conversión del Disco | [`07-localizacion-y-conversion-disco.md`](temario/07-localizacion-y-conversion-disco.md) | Ubicación en libvirt (`domblklist`), copia a `box.img` y compresión QCOW2 (`qemu-img`) |
| **08** | Metadatos del Box | [`08-metadatos-del-box.md`](temario/08-metadatos-del-box.md) | Redacción y validación sintáctica con Python del archivo `metadata.json` |
| **09** | Vagrantfile Interno del Box | [`09-vagrantfile-interno.md`](temario/09-vagrantfile-interno.md) | Parámetros predeterminados del proveedor libvirt incorporados en la plantilla |
| **10** | Empaquetado del Box | [`10-empaquetado-del-box.md`](temario/10-empaquetado-del-box.md) | Construcción del archivo `.box` mediante `tar` y cálculo de hash SHA-256 |
| **11** | Instalación en el Repositorio Local | [`11-instalacion-en-repositorio-local.md`](temario/11-instalacion-en-repositorio-local.md) | Comando `vagrant box add`, verificación del catálogo local con `vagrant box list` |
| **12** | Pruebas de Despliegue | [`12-pruebas-de-despliegue.md`](temario/12-pruebas-de-despliegue.md) | Creación de proyecto independiente, validación SSH, red, DNS y destrucción controlada |
| **13** | Distribución del Box | [`13-distribucion-del-box.md`](temario/13-distribucion-del-box.md) | Compartición mediante fichero, URL, catálogos internos y situación de HCP Registry |
| **14** | Errores Frecuentes y Soluciones | [`14-errores-frecuentes-y-soluciones.md`](temario/14-errores-frecuentes-y-soluciones.md) | Diagnóstico de permisos libvirt, fallos de SSH, red ausente o tamaño excesivo |
| **15** | Automatización con Packer | [`15-automatizacion-con-packer.md`](temario/15-automatizacion-con-packer.md) | Construcción declarativa y reproducible de imágenes mediante el plugin QEMU |
| **16** | Conclusiones y Referencias | [`16-conclusiones-y-referencias.md`](temario/16-conclusiones-y-referencias.md) | Resumen conceptual del proceso y enlaces a documentación técnica oficial |

---

## 🛠️ Especificaciones Técnicas del Laboratorio
- **Sistema Anfitrión:** Linux con virtualización asistida por hardware.
- **Hipervisor / Emulador:** KVM / QEMU.
- **Controlador:** libvirt (`virsh`).
- **Complemento Vagrant:** `vagrant-libvirt`.
- **Sistema Huésped del Box:** Debian 13 (Trixie).
