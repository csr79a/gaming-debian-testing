# Gaming en Debian Testing

Scripts para preparar y limpiar un entorno de gaming en **Debian Testing**, con especial atención a Steam/Proton, GameMode, MangoHud, Protontricks, Heroic y herramientas auxiliares.

El proyecto está pensado para instalar lo necesario sin forzar una instalación de Wine del sistema cuando la combinación disponible en Debian Testing no es adecuada.

## Qué incluye

- Steam.
- Proton y herramientas relacionadas.
- ProtonPlus para gestionar builds de Proton-GE.
- Protontricks mediante `pipx`.
- Winetricks como script oficial.
- GameMode.
- MangoHud compilado desde fuente con soporte NVML cuando corresponde.
- MangoJuice como interfaz gráfica opcional.
- Heroic Games Launcher.
- Lutris y Gamescope como componentes opcionales.
- `game-performance` para ejecutar juegos con el perfil de energía de rendimiento cuando está disponible.
- Configuración de `ntsync`.
- Ajustes del sistema relacionados con juegos modernos.
- Herramientas de diagnóstico como `mesa-utils`.

## Scripts

### Instalación

`setup-gaming-debian-testing.sh`

Instala y configura el entorno de gaming para Debian Testing. El script está diseñado para poder ejecutarse de nuevo sin intentar duplicar configuraciones ya existentes.

### Limpieza

`cleanup-gaming-debian-testing.sh`

Revierte los componentes instalados o configurados por el script de instalación.

Incluye una limpieza específica de lanzadores `.desktop` de Steam que puedan quedar en el menú de aplicaciones después de desinstalar Steam.

> La limpieza no borra por defecto tus juegos ni tus datos personales.

## Requisitos

- Debian Testing.
- Una sesión de usuario normal; los scripts solicitan `sudo` cuando necesitan privilegios.
- Conexión a Internet para descargar paquetes y, cuando corresponda, código fuente.
- Repositorios APT correctamente configurados.
- `pipx` para Protontricks.
- Flatpak es opcional y solo se utiliza para los componentes que el instalador ofrece por esa vía.

## Instalación

Descarga o clona el repositorio y ejecuta:

```bash
chmod +x setup-gaming-debian-testing.sh
./setup-gaming-debian-testing.sh
```

El script muestra cada bloque y continúa según las opciones disponibles en el sistema.

## Comprobación posterior

Después de la instalación se pueden comprobar algunos componentes con:

```bash
steam
gamemoderun true
gamescope --version
mangohud --version
protontricks --version
winetricks --version
game-performance --help
```

Para Protontricks, que no aparezcan juegos inmediatamente no significa necesariamente que exista un problema: debe existir al menos un juego de Steam ejecutado mediante Proton para que Protontricks pueda detectarlo.

## Wine y Winetricks

El proyecto no fuerza la instalación de Wine del sistema.

En Debian Testing puede ocurrir que no exista una combinación de paquetes Wine nativos adecuada para el sistema actual. Steam utiliza su propia infraestructura Proton para los juegos compatibles.

Por ese motivo:

- Protontricks se utiliza para juegos de Steam/Proton.
- Winetricks se mantiene como herramienta disponible para prefijos Wine tradicionales.
- Si no hay `wine`/`wineserver` del sistema, Winetricks puede mostrar un aviso y no podrá gestionar prefijos Wine normales.
- Esto no impide utilizar Steam + Proton.

## Steam y el icono que queda después de desinstalar

La limpieza contempla dos situaciones diferentes:

1. El paquete de Steam sigue instalado.
2. El paquete ya fue eliminado, pero quedó un lanzador `.desktop` de usuario.

El limpiador comprueba estos lanzadores conocidos:

```text
~/.local/share/applications/steam.desktop
~/.local/share/applications/steam-native.desktop
~/.local/share/applications/steam-url-handler.desktop
~/.local/share/applications/steam-launcher.desktop
```

Si existen, el script muestra cuáles encontró y ofrece eliminarlos.

No elimina automáticamente:

```text
~/.steam
~/.local/share/Steam
~/Games
```

ni otros datos de juegos.

## Limpieza

Primero se recomienda una simulación:

```bash
chmod +x cleanup-gaming-debian-testing.sh
./cleanup-gaming-debian-testing.sh --dry-run
```

Para ejecutar la limpieza interactiva:

```bash
./cleanup-gaming-debian-testing.sh
```

Para aceptar las confirmaciones normales:

```bash
./cleanup-gaming-debian-testing.sh -y
```

Para ofrecer además el borrado de datos personales de juegos:

```bash
./cleanup-gaming-debian-testing.sh --purge-data
```

`--purge-data` es deliberadamente más restrictivo y requiere una confirmación explícita escribiendo `BORRAR`.

## Qué NO hace la limpieza por defecto

El limpiador no elimina:

- Tus juegos.
- Tus partidas locales.
- `~/.steam`.
- `~/.local/share/Steam`.
- `~/Games`.
- La arquitectura `i386`.
- `power-profiles-daemon`.
- Flatpak ni el remoto Flathub.
- Paquetes de desarrollo que puedan ser utilizados por otros programas.

## Estructura del proyecto

```text
.
├── README.md
├── setup-gaming-debian-testing.sh
├── cleanup-gaming-debian-testing.sh
└── docs
    ├── README.md
    ├── MANUAL.md
    ├── INSTALACION.md
    └── DESINSTALACION.md
```

## Documentación

- [Manual completo](docs/MANUAL.md)
- [Instalación](docs/INSTALACION.md)
- [Desinstalación y limpieza](docs/DESINSTALACION.md)
- [Documentación de `docs/`](docs/README.md)

## Filosofía del proyecto

El objetivo no es instalar el máximo número posible de paquetes, sino utilizar las herramientas que tienen sentido en Debian Testing y evitar forzar componentes que puedan introducir conflictos de dependencias.

En particular, el instalador no convierte una instalación de Debian Testing en una instalación basada en paquetes de otra versión de Debian solo para obtener Wine.

## Seguridad

Los scripts deben ejecutarse como usuario normal, no como `root`.

Antes de ejecutar una limpieza destructiva:

```bash
./cleanup-gaming-debian-testing.sh --dry-run
```

Revisa la lista que muestra el script antes de aceptar cualquier operación.

## Licencia

Añade aquí la licencia que corresponda al repositorio si todavía no está definida.
