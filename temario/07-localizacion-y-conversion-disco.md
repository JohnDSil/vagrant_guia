# 07. Localización, Copia y Conversión del Disco Virtual

Con la máquina apagada, se localiza el disco administrado por libvirt en el host para prepararlo y comprimirlo.

## 7.1 Identificación del Disco Virtual en Libvirt
Listar las máquinas definidas en el anfitrión:
```bash
virsh -c qemu:///system list --all
```

Consultar la ruta exacta de los bloques de disco asociados a la máquina base:
```bash
sudo virsh -c qemu:///system domblklist NOMBRE_MAQUINA --details
```
*(Sustituir `NOMBRE_MAQUINA` por el nombre que reporte libvirt. Habitualmente los discos se alojan bajo `/var/lib/libvirt/images/`).*

## 7.2 Copia del Disco al Directorio del Box
Crear un subdirectorio en el anfitrión para ensamblar el box y copiar la imagen con el nombre estándar `box.img`:
```bash
mkdir -p ~/vagrant-box-debian13/box
sudo cp /var/lib/libvirt/images/debian13-base.qcow2 ~/vagrant-box-debian13/box/box.img
```

Verificar la existencia de la copia:
```bash
ls -lh ~/vagrant-box-debian13/box/box.img
```
*Si el archivo es propiedad de root, se pueden ajustar los permisos de la copia en el directorio de trabajo (nunca los del disco original).*

## 7.3 Conversión y Compresión QCOW2
Para optimizar el tamaño del fichero a empaquetar, convertir la imagen activando compresión con `qemu-img`:
```bash
cd ~/vagrant-box-debian13/box
qemu-img convert -c -O qcow2 box.img box-comprimido.img
```

Verificar la imagen generada y sustituir la copia sin comprimir:
```bash
qemu-img info box-comprimido.img
mv box-comprimido.img box.img
```

Consultar la capacidad virtual del disco (valor requerido para el metadato):
```bash
qemu-img info box.img
```
*El parámetro `virtual size` indica la capacidad total asignada al disco (por ejemplo, 20 GiB), independientemente del espacio físico que ocupe el fichero comprimido en el anfitrión.*
