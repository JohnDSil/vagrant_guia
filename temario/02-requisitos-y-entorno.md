# 02. Requisitos Previos y Preparación del Sistema Anfitrión

Para poder generar y ejecutar boxes para libvirt, el equipo anfitrión (Linux) debe soportar extensiones de virtualización por hardware.

## 2.1 Verificación de Arquitectura y Virtualización
Comprobar la arquitectura del procesador (debe ser x86_64):
```bash
uname -m
