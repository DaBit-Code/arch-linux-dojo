🏯 2. Particionado con cfdisk

cfdisk /dev/sda

Selecciona el tipo de tabla de particiones (GPT o DOS según tu VM).

Crea las siguientes particiones:

    swap → 2 GB

    root → 20 GB

    home → el resto del espacio

Guarda los cambios y sal.
