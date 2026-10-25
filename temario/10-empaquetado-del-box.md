# 10. Empaquetado del Vagrant Box

## 10.1 Comprobación de Archivos Requeridos
Antes de construir el paquete, comprobar que la carpeta contiene exactamente los tres elementos requeridos:
```bash
cd ~/vagrant-box-debian13/box
ls -lh
```
Deben estar presentes:
1. `box.img`
2. `metadata.json`
3. `Vagrantfile`

## 10.2 Generación del Paquete Comprimido
Empaquetar los tres archivos mediante `tar` con compresión gzip para generar el fichero `.box`:
```bash
tar -czf ../debian13-libvirt.box metadata.json Vagrantfile box.img
```

Verificar el contenido empaquetado sin descomprimirlo:
```bash
tar -tzf ../debian13-libvirt.box
```
*Debe listar los tres archivos incluidos.*

## 10.3 Generación del Hash de Integridad SHA-256
Calcular la suma de verificación para comprobar la integridad del paquete si se transfiere a otros equipos:
```bash
sha256sum ../debian13-libvirt.box
```
*Guardar el valor obtenido junto al archivo generado.*
