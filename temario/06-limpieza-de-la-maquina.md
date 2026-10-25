# 06. Limpieza, Optimización y Apagado de la Máquina

Antes de extraer el disco virtual, deben eliminarse archivos temporales, registros y cachés para minimizar el tamaño de la plantilla y no transferir credenciales previas.

## 6.1 Limpieza de la Caché de APT
Ejecutar dentro de la máquina base:
```bash
sudo apt clean
sudo apt autoremove --purge -y
```

## 6.2 Eliminación de Archivos Temporales
Vaciar las áreas de almacenamiento temporal del sistema operativo invitado:
```bash
sudo find /tmp -mindepth 1 -maxdepth 1 -exec rm -rf -- {} +
sudo find /var/tmp -mindepth 1 -maxdepth 1 -exec rm -rf -- {} +
```
> **Nota:** Estos comandos deben ejecutarse dentro de la máquina virtual, nunca en el anfitrión.

## 6.3 Limpieza de Historiales y Credenciales
Eliminar los historiales de shell de los usuarios utilizados:
```bash
rm -f ~/.bash_history
sudo rm -f /root/.bash_history
```
*Comprobar además que no existan ficheros con claves privadas, tokens o configuraciones personales residuales.*

## 6.4 Liberación de Bloques Libres en Disco
Para optimizar el tamaño de la imagen QCOW2 final, descartar los bloques no utilizados en el sistema de ficheros:
```bash
sudo fstrim -av
```
*(Si la configuración del almacenamiento virtual no admite descarte, la operación no tendrá efecto. Aunque existe la técnica de rellenar con ceros mediante un fichero temporal, consume mucho espacio físico y tiempo, por lo que no es obligatoria).*

## 6.5 Apagado Ordenado de la Máquina
Cerrar la sesión SSH:
```bash
exit
```

Desde el anfitrión, ordenar la parada completa de la máquina gestionada por Vagrant:
```bash
vagrant halt
vagrant status
```
> **Aviso Crítico:** Nunca se debe copiar ni transformar el disco virtual mientras la máquina esté en ejecución, ya que la imagen resultaría inconsistente o corrupta.
