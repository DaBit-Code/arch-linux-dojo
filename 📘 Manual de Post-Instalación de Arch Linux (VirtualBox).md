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

Para instalar Ulauncher necesitamos hacer esto ya que no es un repositorio oficial de Arch Linux

    sudo pacman -S git base-devel
    git clone https://aur.archlinux.org/yay.git
    cd yay
    makepkg -si

Instalar Ulauncher desde AUR

    yay -S ulauncher

Una vez terminada la instalación habilita el servicio para que arranque automaticamente

    systemctl --user enable --now ulauncher.service

Para evitar problemas de Hoykey has lo siguiente

Edita el archivo de configuración de Ulauncher:

    vim ~/.config/ulauncher/settings.json

Busca la clave "hotkey_show_app" y cámbiala a algo inválido o vacío:

    "hotkey_show_app": ""

Instala Polybar y Ulauncher:

    pacman -S polybar ulauncher

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
    #Integrar Ulauncher a BSPWM (agregala debajo de pgrep -x sxhkd > /dev/null || sxhkd &)
    pgrep -x ulauncher > /dev/null || ulauncher & 
    #Integrar Polybar a BSPWM
    polybar main &

Edita ~/.config/sxhkd/sxhkdrc y añade:

    vim ~/.config/sxhkd/sxhkdrc
    alt + Return
        kitty
    alt + d
        ulauncher-toggle
Nota: “En algunos entornos virtualizados (VirtualBox), la tecla Super puede no transmitirse correctamente. Si los atajos no responden, prueba con Alt o Ctrl como sustituto temporal”

Configuracion básica de la Polybar

    mkdir -p ~/.config/polybar

Edita ~/.config/polybar/config.ini

    vim ~/.config/polybar/config.ini
    [bar/main]
    width = 100%
    height = 24
    background = #222
    foreground = #fff

    modules-left = xworkspaces
    modules-right = date

    [module/xworkspaces]
    type = internal/xworkspaces

    [module/date]
    type = internal/date
    interval = 5
    date = %H:%M

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

## Apéndice: Errores comunes y rescates rápidos

### Pantalla negra al iniciar BSPWM
- **Causa:** `~/.xinitrc` mal configurado o `bspwmrc` no ejecutable.
- **Rescate:**
  ```bash
  echo "exec bspwm" > ~/.xinitrc
  chmod +x ~/.config/bspwm/bspwmrc


✅ Resultado

Al iniciar sesión como usuario dojo y ejecutar startx, tendrás:

    BSPWM como gestor de ventanas.

    Polybar mostrando escritorios y módulos.

    Ulauncher como lanzador de aplicaciones (alt + D).

    Kitty como terminal (alt + Return).

    Tmux para sesiones persistentes y paneles dentro de Kitty.

Nota:

    root solo se usa para instalar y configurar servicios, mientras que usuario dojo es el entorno de trabajo diario
