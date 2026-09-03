# Pulpo Custom Debloat

Herramienta PowerShell 5.1 + WPF para aplicar una optimizacion repetible en equipos con Windows 10 y Windows 11.

## Ejecucion remota

Abre Windows PowerShell y ejecuta:

```powershell
irm https://raw.githubusercontent.com/brunosantunez/pulpo_custom_debloat/main/Install.ps1 | iex
```

El instalador descarga la rama `main` desde GitHub, valida los archivos principales y abre la interfaz. No requiere alojamiento adicional.

## Uso

1. Ejecuta `Run.cmd`.
2. Acepta la elevacion de administrador.
3. Elige `Basica segura`, `Taller completo` o selecciona acciones manualmente.
4. Revisa la previsualizacion y pulsa `Aplicar seleccion`.

Antes de cada aplicacion se crea obligatoriamente un punto de restauracion llamado `Revertir cambios - Pulpo Custom Debloat`. Si Windows no permite crearlo, el proceso se detiene sin aplicar ajustes.

## Perfiles

- `Basica segura`: privacidad, sugerencias, busqueda web, notificaciones, acceso remoto, dispositivos moviles, drivers por Windows Update, Game Bar, plan de energia y limpieza. Conserva componentes con impacto funcional alto.
- `Taller completo`: agrega hibernacion, almacenamiento reservado, OneDrive, IA, aplicaciones incluidas con Windows, Xbox completo y servicios opcionales relacionados.
- `Captura agresiva`: reproduce los servicios no esenciales de la captura. Puede desactivar impresion, camara, busqueda, mandos, hotspot y actualizaciones de Edge. Requiere una confirmacion adicional.

`Display Policy Service` y `Group Policy Client` estan protegidos. Windows Defender, Windows Update, BITS, Microsoft Store, App Installer, Windows Security, el shell y sus runtimes tambien quedan fuera del alcance.

El asistente de concentracion no se modifica escribiendo datos binarios internos de `CloudStore`: ese formato cambia entre versiones y no ofrece una politica estable comun a Windows 10 y 11. El perfil ya desactiva las notificaciones globales solicitadas.

La reversion interna restaura valores de registro, servicios y el plan de energia desde la ultima sesion. Las aplicaciones eliminadas y los archivos temporales borrados no tienen una reversion interna fiable; para eso se incluye el boton `Abrir Restaurar sistema`.

## Validacion

Ejecuta:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\tests\Validate.ps1
```

El proyecto toma como referencia los enfoques publicos de [WinUtil](https://github.com/ChrisTitusTech/winutil), [Win11Debloat](https://github.com/Raphire/Win11Debloat) y [FPSBoostPro](https://github.com/itechfever/FPSBoostPro). La implementacion de este repositorio es independiente y mantiene sus propias listas de seguridad.
