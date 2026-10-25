# 13. Distribución del Vagrant Box

Una vez validada la plantilla, puede compartirse con otros usuarios mediante distintos mecanismos.

## 13.1 Distribución Mediante Archivo Directo
Consiste en transferir el archivo `debian13-libvirt.box` a través de un servidor de ficheros, almacenamiento compartido o dispositivo externo. El receptor solo debe ejecutar:
```bash
vagrant box add \
  --name debian13-libvirt \
  --provider libvirt \
  debian13-libvirt.box
```
*Se recomienda verificar previamente el hash SHA-256 para constatar que el fichero no se ha corrompido durante la transferencia.*

## 13.2 Distribución Mediante Servidor Web (URL)
Si el archivo `.box` se aloja en un servidor web accesible (preferiblemente con protocolo HTTPS), puede añadirse directamente sin descargarlo previamente de forma manual:
```bash
vagrant box add \
  --name debian13-libvirt \
  --provider libvirt \
  [https://servidor.example/boxes/debian13-libvirt.box](https://servidor.example/boxes/debian13-libvirt.box)
```

## 13.3 Catálogos y Situación de HCP Vagrant Registry
Los catálogos permiten versionar imágenes y asociar múltiples proveedores a un mismo identificador. 

Debido a que **HCP Vagrant Registry** cerró la creación de nuevos boxes en octubre de 2026 y fija su descontinuación para diciembre de 2026, los nuevos entornos no deben depender de este servicio. Para entornos educativos o corporativos se recomienda alojar los paquetes y sus metadatos en infraestructura propia o compartirlos como ficheros directos.
