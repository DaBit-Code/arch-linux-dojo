Antes de iniciar con la instalación de programas y paquetes verifica que tengas navegación hacia Internet

    ping 8.8.8.8

Si no tienes navegación por favor haz lo siguiente

    Inicia de nuevo con el LiveCD de Arch Linux

Monta la raíz y activa el swap

    mount /dev/sda2 /mnt
    swapon /dev/sda1

Entrar al sistema con

    arch-chroot /mnt

Instala o reinstala los paquetes necesarios (no olvides habilitarlos)

    pacman -S networkmanager virtualbox-guest-utils
    systemctl enable NetworkManager
    systemctl enable vboxservice

Salte con

    exit

Apaga la Maquina Virtual, desmonta el LiveCD para que inicie desde el Disco Duro haz login con root y la contraseña que elegiste (si seguiste el manual tal cual la contraseña es dojo) y prueba de nuevo si tienes salida hacia Internet con

    ping 8.8.8.8

Si tienes respuesta del ping continua con la instalación de programas y paquetes.
    
Instala utilidades esenciales:

    pacman -S sudo git base-devel

Crear usuario dojo (root)

    useradd -m -G wheel -s /bin/bash dojo
    passwd dojo
    # (contraseña: dojo)

 Editar sudoers para dar permisos al grupo wheel:

    EDITOR=vim visudo

Descomenta la línea:

    %wheel ALL=(ALL:ALL) ALL

Instalar entorno gráfico mínimo (root)

    pacman -S xorg xorg-xinit
    
Instala BSPWM y SXHKD:

    pacman -S bspwm sxhkd

Instala Polybar y Rofi:

    pacman -S polybar rofi

Instala Kitty (terminal moderna):

    pacman -S kitty

Instala Tmux (multiplexor):

    pacman -S tmux

Nota:

    Salir de root a dojo: exit y luego iniciar sesión como dojo.

    Entrar de dojo a root: su - (contraseña de root).

    Método recomendado: usar sudo desde dojo para tareas administrativas.

Configuración inicial (usuario dojo)
Copia configuraciones de ejemplo:

    mkdir -p ~/.config/bspwm ~/.config/sxhkd
    cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/
    cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/
    chmod +x ~/.config/bspwm/bspwmrc

Edita ~/.config/bspwm/bspwmrc y añade al final:

    vim ~/.config/bspwm/bspwmrc
    polybar main &

Edita ~/.config/sxhkd/sxhkdrc y añade:

    vim ~/.config/sxhkd/sxhkdrc
    super + Return
        kitty
    super + d
        rofi -show drun

Fuentes y apariencia (root)

    pacman -S ttf-dejavu ttf-font-awesome

Arranque gráfico (usuario dojo)

    echo "exec bspwm" > ~/.xinitrc

Inicia entorno gráfico:
    
    startx

Multiplexor Tmux (usuario dojo)

    tmux

Opcional: instalar configuración optimizada Oh My Tmux:

    git clone https://github.com/gpakosz/.tmux.git ~/.tmux
    ln -s -f ~/.tmux/.tmux.conf ~/.tmux.conf
    cp ~/.tmux/.tmux.conf.local ~/

✅ Resultado

Al iniciar sesión como usuario dojo y ejecutar startx, tendrás:

    BSPWM como gestor de ventanas.

    Polybar mostrando escritorios y módulos.

    Rofi como lanzador de aplicaciones (Super + D).

    Kitty como terminal (Super + Return).

    Tmux para sesiones persistentes y paneles dentro de Kitty.

Nota:

    root solo se usa para instalar y configurar servicios, mientras que usuario dojo es el entorno de trabajo diario
