Personalizar Kitty

Lo que vamos a definir en Kitty:

- Fuente: usar Nerd Fonts para iconos y ligaduras.

- Colores: esquema tipo Dracula, Gruvbox o Nord.

- Transparencia: con Picom activo.

- Atajos: scroll, copiar/pegar, tamaño de fuente.

Crea el archivo kitty.conf

    vim ~/.config/kitty/kitty.conf

Pega esto como configuracion minima (puedes personalizar a tu gusto si deseas)

    # Fuente
    font_family      JetBrainsMono Nerd Font
    font_size        12.0

    # Colores (ejemplo Gruvbox)
    foreground       #ebdbb2
    background       #282828
    color0           #282828
    color1           #cc241d
    color2           #98971a
    color3           #d79921
    color4           #458588
    color5           #b16286
    color6           #689d6a
    color7           #a89984

    # Transparencia (requiere picom)
    background_opacity 0.90

    # Atajos
    map ctrl+shift+c copy_to_clipboard
    map ctrl+shift+v paste_from_clipboard

  ✅ Resultado esperado

  - Kitty con fuente clara y soporte de iconos.
  - Colores consistentes (tema elegido).
  - Transparencia ligera para estética.
  - Atajos básicos configurados.

Estética de Polybar

Lo que vamos a definir:

    Fuente e iconos → usar Nerd Fonts para que aparezcan símbolos bonitos (wifi, batería, volumen).

    Colores → esquema consistente con Kitty (ej. Gruvbox, Nord, Dracula).

    Módulos útiles → hora, red, volumen, batería, temperatura.

    Transparencia → opcional, con Picom activo.

Ejemplo mínimo en ~/.config/polybar/config.ini:
