Manual de Instalación de Arch Linux 🥷
Este documento guía paso a paso la instalación de Arch Linux desde cero, incluyendo errores comunes que suelen enfrentar los principiantes. El corte llega hasta el primer arranque exitoso con GRUB.

Índice
Preparación del entorno
Particionado del disco
Formateo y montaje
Instalación del sistema base
Configuración inicial
Kernel e initramfs
Bootloader (GRUB)
Primer arranque
Errores comunes documentados

1. Preparación del entorno
Descargar ISO de Arch Linux.
Crear VM en VirtualBox (mínimo 40 GB de disco).
Verificar conexión a internet:
ping -c 3 archlinux.org

2. Particionado del disco
Usar cfdisk:
cfdisk /dev/sda
Ejemplo esquema MBR:
/dev/sda1 → 2 GB, tipo 82 Linux Swap
/dev/sda2 → 20 GB, tipo 83 Linux (root /)
/dev/sda3 → 18 GB, tipo 83 Linux (home /home)

3. Formateo y montaje
mkswap /dev/sda1
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3

swapon /dev/sda1
mount /dev/sda2 /mnt
mkdir /mnt/home
mount /dev/sda3 /mnt/home

4. Instalación del sistema base
Actualizar mirrors:
reflector --country Mexico --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
pacman -Syy
Instalar base:
pacstrap /mnt base linux linux-firmware vim
Generar fstab:
genfstab -U /mnt >> /mnt/etc/fstab
Entrar al nuevo sistema:
arch-chroot /mnt

5. Configuración inicial
Zona horaria:
ln -sf /usr/share/zoneinfo/America/Merida /etc/localtime
hwclock --systohc
Locales:
vim /etc/locale.gen   # Descomentar en_US.UTF-8 y es_MX.UTF-8
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
Hostname:
echo dojo > /etc/hostname
Hosts:
cat >> /etc/hosts <<EOF
127.0.0.1   localhost
::1         localhost
127.0.1.1   dojo.localdomain dojo
EOF
Contraseña root (¡no olvidar este paso!):
passwd

6. Kernel e initramfs
Instalar kernel:
pacman -S linux linux-firmware
Nota: Pacman preguntará por proveedor de initramfs. Elegir 1) mkinitcpio.
Regenerar imágenes:
mkinitcpio -P
Si aparece error file not found '/etc/vconsole.conf', crear el archivo:
echo KEYMAP=us > /etc/vconsole.conf
mkinitcpio -P

7. Bootloader (GRUB)
Instalar GRUB:
pacman -S grub os-prober
Verificar si VM está en BIOS o UEFI:
ls /sys/firmware/efi/efivars
Si existe → UEFI.
Si no → BIOS/MBR.
BIOS/MBR:
grub-install --target=i386-pc /dev/sda
UEFI:
pacman -S efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Arch
Generar configuración:
grub-mkconfig -o /boot/grub/grub.cfg

8. Primer arranque
Salir del chroot:
exit
Desmontar particiones:
umount -R /mnt
Apagar VM y quitar ISO de VirtualBox.
Reiniciar: GRUB debe aparecer y permitir iniciar sesión como root con la contraseña configurada.

9. Errores comunes documentados
pacstrap: Failed to install packages to new root → probar primero pacstrap /mnt base y luego instalar kernel dentro del chroot.
mkinitcpio: file not found '/etc/vconsole.conf' → crear /etc/vconsole.conf con KEYMAP=us o la-latin1.
Olvidar contraseña root → arrancar con ISO, montar /mnt, entrar al chroot y ejecutar passwd.
GRUB no arranca → verificar si VM está en BIOS o UEFI y usar el comando correcto.

✅ Con esto tu Arch Linux queda instalado y arranca con GRUB. El siguiente manual cubrirá la post‑instalación (usuarios, sudo, NetworkManager, entorno gráfico, BSPWM, Kitty, Tmux).

