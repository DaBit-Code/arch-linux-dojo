# Fase 1: Particionado y Base System 🏯

En esta fase preparamos el disco, montamos las particiones y dejamos instalado el sistema base de Arch Linux.  
Usamos **cfdisk** en lugar de fdisk, ya que ofrece una interfaz más clara para capturas de pantalla y documentación.

---

## 🏯 1. Verificar discos
```bash
lsblk

Esto muestra los discos disponibles. En este dojo usamos /dev/sda.
