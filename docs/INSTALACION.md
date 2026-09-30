# Instalación de Gaming en Debian Testing

## 1. Requisitos
- Debian Testing.
- Usuario normal con `sudo`.
- Conexión a Internet.
- Repositorios APT funcionales.
- Para los componentes Flatpak, acceso a Flathub.

El script comprueba que las fuentes Debian apunten a Testing y no convierte otras suites automáticamente.

## 2. Instalación
~~~bash
chmod +x setup-gaming-debian-testing.sh
./setup-gaming-debian-testing.sh
~~~

No ejecutes el script como `root`.

## 3. Qué hace
El instalador prepara, según disponibilidad y opciones:
- Steam.
- Flatpak/Flathub y ProtonPlus.
- Heroic Games Launcher.
- GameMode.
- MangoHud compilado con soporte NVML cuando corresponde.
- MangoJuice opcional.
- Winetricks y Protontricks.
- configuración de GameMode y MangoHud.
- `vm.max_map_count`.
- `ntsync`.
- `game-performance`.
- Lutris y Gamescope opcionales.

## 4. Después de instalar
~~~bash
steam
gamemoderun true
gamescope --version
mangohud --version
protontricks --version
winetricks --version
command -v game-performance
~~~

Para Protontricks, ejecuta al menos una vez un juego de Steam mediante Proton antes de esperar que aparezca en su lista.

## 5. Repetir la instalación
El script está diseñado para poder ejecutarse de nuevo. Las configuraciones marcadas por el proyecto se reconocen y no se sobrescriben innecesariamente.

Si existe una configuración externa sin la marca del proyecto, se conserva para evitar pisarla.

## 6. Más información
Consulta `docs/MANUAL.md` para el funcionamiento detallado y `docs/DESINSTALACION.md` para la limpieza.