Configuración del teclado en Arch

Configuración temporal (solo para probar)

En tu sesión actual (TTY o Kitty):

    setxkbmap latam

Configuración persistente en Xorg

       sudo vim /etc/X11/xorg.conf.d/00-keyboard.conf

Nota: Tienes que cambiarte a usuario root

        su -
        
Con este contenido:

    Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
        Option "XkbLayout" "latam"
    EndSection
Luego regresar al usuario normal con

    exit
    
Configuración persistente en consola (TTY)

Si también quieres que el teclado esté en latam en TTY (fuera de Xorg):

    sudo localectl set-keymap latam
    
Verificación

- En Kitty, prueba las teclas (ñ, acentos, símbolos).

- En TTY, prueba que también esté en latam.

- Si algo falla, revisa con:

      localectl status
      #Debe mostrar X11 Layout: latam.
     
