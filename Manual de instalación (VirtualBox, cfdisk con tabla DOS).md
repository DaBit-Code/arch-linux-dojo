Configuracion de minima en Virtual Box:

    4 Gb en RAM
    2 Procesadores  
    40 Gb en Disco Duro
    Red en NAT

Particionado guiado con cfdisk

Abrir el gestor:

    cfdisk /dev/sda

Elegir tipo de tabla:

    Selecciona dos (MBR) como etiqueta de particiones.

Crear swap (2G):

    New → tamaño 2G → Primary → Type → Linux swap / Solaris.

Crear raíz (20G):

    Selecciona Free space → New → tamaño 20G → Primary → deja el Type en Linux.

Guardar cambios:

    Write → escribe “yes” → Quit.

 Formateo y montaje:
 
    mkswap /dev/sda1
    swapon /dev/sda1
    mkfs.ext4 /dev/sda2
    mount /dev/sda2 /mnt

Base e ingreso al sistema:

    pacstrap /mnt base

Teclado antes del chroot:

    echo KEYMAP=us > /mnt/etc/vconsole.conf
    # o
    echo KEYMAP=latam > /mnt/etc/vconsole.conf

Entrar al sistema:

    arch-chroot /mnt

Kernel, initramfs y utilidades

    pacman -S linux linux-firmware vim
    mkinitcpio -P

Identidad del sistema
Hostname:

    echo dojo > /etc/hostname

Hosts (edita con vim):

    vim /etc/hosts

Añade o verifica que queden estas líneas:

    127.0.0.1   localhost
    ::1         localhost
    127.0.1.1   dojo.localdomain dojo

GRUB en BIOS/MBR

    pacman -S grub
    grub-install --target=i386-pc /dev/sda
    grub-mkconfig -o /boot/grub/grub.cfg

Nota:

    Debes apagar la maquina virtual, eliminar la ISO para que arranque directamente desde el Disco Duro y volver arrancas la     maquina virtual
    
Si todo sale bien deberías ver:
    
    GRUB y arrancar tu Arch.
