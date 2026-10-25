# 08. Creación del Archivo de Metadatos (`metadata.json`)

El archivo `metadata.json` define los atributos esenciales que permiten a Vagrant identificar el hipervisor compatible y la capacidad del disco.

## 8.1 Creación del Fichero
Dentro del directorio `~/vagrant-box-debian13/box/`, crear el archivo:
```bash
cd ~/vagrant-box-debian13/box
nano metadata.json
```

Añadir la estructura JSON con los valores del entorno:
```json
{
  "provider": "libvirt",
  "format": "qcow2",
  "virtual_size": 20
}
```

- `"provider": "libvirt"`: Indica que el box solo debe ejecutarse mediante el complemento vagrant-libvirt.
- `"format": "qcow2"`: Formato nativo del disco virtual.
- `"virtual_size"`: Capacidad virtual del disco expresada en gigabytes enteros (debe coincidir con la capacidad obtenida mediante `qemu-img info`, no con el peso del archivo físico).

## 8.2 Validación Sintáctica
Verificar que el archivo JSON está correctamente formateado y carece de errores sintácticos:
```bash
python3 -m json.tool metadata.json
```
*Si no devuelve errores, el archivo es válido.*
