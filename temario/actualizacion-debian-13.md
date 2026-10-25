# 04. Actualización a Debian 13 (Trixie)

Este procedimiento se ejecuta dentro de la máquina virtual cuando la plantilla de partida está basada en Debian 12 (Bookworm).

## 4.1 Puesta al Día de Debian 12
Antes de cambiar de rama, actualizar por completo el sistema actual:
```bash
sudo apt update
sudo apt full-upgrade -y
```

## 4.2 Reemplazo de Fuentes de Repositorios (APT)
1. Realizar una copia de seguridad del directorio de configuración de paquetes:
```bash
sudo cp -a /etc/apt /etc/apt.backup
```

2. Localizar dónde están definidas las fuentes de software (`sources.list` o archivos deb822 en `sources.list.d/`):
```bash
grep -R -nE 'bookworm|trixie' /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
```

3. Modificar los repositorios oficiales cambiando la rama `bookworm` por `trixie` (incluyendo actualizaciones de seguridad y paquetes base).
> **Precaución:** No reemplazar indiscriminadamente todos los orígenes. Deshabilitar o comprobar repositorios de terceros para evitar bloqueos por incompatibilidad.

4. Actualizar el índice de paquetes contra los nuevos repositorios:
```bash
sudo apt update
```

## 4.3 Ejecución de la Actualización y Reinicio
Aplicar la actualización del sistema en dos fases secuenciales:
```bash
# 1. Actualización básica sin incorporar nuevos paquetes
sudo apt upgrade --without-new-pkgs

# 2. Actualización completa con resolución de dependencias
sudo apt full-upgrade
```

Purgar paquetes obsoletos o dependencias huérfanas:
```bash
sudo apt autoremove --purge -y
```

Reiniciar el sistema para cargar el nuevo kernel:
```bash
sudo reboot
```

## 4.4 Verificación
Volver a conectar por SSH desde el anfitrión:
```bash
vagrant ssh
```

Comprobar que el sistema operativo se ha actualizado correctamente:
```bash
cat /etc/os-release
```
*El resultado debe confirmar `Debian GNU/Linux 13 (Trixie)`. Verificar también que la conectividad de red y SSH funcionan con normalidad.*
