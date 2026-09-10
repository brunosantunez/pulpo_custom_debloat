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

La aplicacion solicita elevacion normal de administrador y usa Windows PowerShell 5.1 nativo en modo STA. Antes de abrir la interfaz pregunta si puede preparar los scripts para el proceso actual; el boton `Herramientas > Preparar scripts` permite repetir esa preparacion. No desactiva UAC, Microsoft Defender ni cambia la politica de ejecucion permanente. Las directivas de grupo restrictivas se informan como error y no se alteran.

Los errores del proceso permanecen en `Restaurar y registro`. Las solicitudes, resultados y archivos `stderr.log` quedan en `%ProgramData%\PulpoCustomDebloat\Requests`; los errores de arranque se guardan en `%LOCALAPPDATA%\PulpoCustomDebloat\Logs`.

## Perfiles

- `Basica segura`: privacidad, sugerencias, busqueda web, notificaciones, acceso remoto, dispositivos moviles, drivers por Windows Update, Game Bar, plan de energia y limpieza. Conserva componentes con impacto funcional alto.
- `Taller completo`: agrega hibernacion, almacenamiento reservado, OneDrive, IA, aplicaciones incluidas con Windows, Xbox completo y servicios opcionales relacionados.
- `Captura agresiva`: reproduce los servicios no esenciales de la captura. Puede desactivar impresion, camara, busqueda, mandos, hotspot y actualizaciones de Edge. Requiere una confirmacion adicional.

`Display Policy Service` y `Group Policy Client` estan protegidos. Windows Defender, Windows Update, BITS, Microsoft Store, App Installer, Windows Security, el shell y sus runtimes tambien quedan fuera del alcance.

El asistente de concentracion no se modifica escribiendo datos binarios internos de `CloudStore`: ese formato cambia entre versiones y no ofrece una politica estable comun a Windows 10 y 11. El perfil ya desactiva las notificaciones globales solicitadas.

La reversion interna restaura valores de registro, servicios y el plan de energia desde la ultima sesion. No recupera aplicaciones eliminadas ni temporales borrados. El boton `Abrir Restaurar sistema` ofrece una recuperacion adicional del sistema, pero no sustituye un respaldo de archivos ni garantiza reinstalar todos los paquetes.

## Validacion

Ejecuta:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\tests\Validate.ps1
powershell.exe -NoProfile -STA -ExecutionPolicy Bypass -File .\tests\ConsentUi.ps1
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\tests\WorkerIntegration.ps1
powershell.exe -NoProfile -STA -ExecutionPolicy Bypass -File .\tests\SmokeUi.ps1
```

Estas pruebas validan carga de modulos, comunicacion concurrente entre procesos, preparacion de scripts limitada a la sesion, interfaz y errores del worker. No aplican perfiles ni prueban cambios reales del sistema. La aplicacion completa y su reversion deben verificarse en una VM con respaldo antes de usarlas en equipos de clientes.

El proyecto toma como referencia los enfoques publicos de [WinUtil](https://github.com/ChrisTitusTech/winutil), [Win11Debloat](https://github.com/Raphire/Win11Debloat) y [FPSBoostPro](https://github.com/itechfever/FPSBoostPro). La implementacion de este repositorio es independiente y mantiene sus propias listas de seguridad.
