# Configuración global de OMP

Exportación de configuración para Oh My Pi (`omp`). Incluye `config.yml`, `mcp.json`, la extensión personalizada de Obsidian y el inventario de plugins de [`plugins.json`](plugins.json). No incluye credenciales, bases de datos, sesiones ni los paquetes instalados de los plugins.

## Instalación

### Requisitos

- Oh My Pi instalado y disponible como comando `omp`.
- Git para clonar este repositorio.
- Node.js con `npx` para los servidores MCP que se ejecutan mediante `npx` y para instalar los plugins npm.
- Instala aparte los ejecutables MCP que vayas a usar: `codegraph`, `sonar` y, opcionalmente, `open-computer-use`. En macOS, instala Xcode para disponer de `xcrun mcpbridge`.
- Para usar los MCP de Figma, Obsidian y SonarQube, prepara sus aplicaciones, vault o autenticación local por separado.

### 1. Obtener este repositorio

Cuando el repositorio esté publicado, clónalo usando su URL HTTPS o SSH. Reemplaza `URL_HTTPS_O_SSH_DEL_REPOSITORIO` por la URL real:

```bash
git clone "URL_HTTPS_O_SSH_DEL_REPOSITORIO" omp-config
cd omp-config
```

Si ya tienes una copia local, entra a su carpeta en lugar de clonarlo.

### 2. Respaldar e instalar la configuración

Este paso conserva una copia local de los archivos que reemplazarás. El respaldo no se agrega al repositorio:

```bash
DEST="$HOME/.omp/agent"
BACKUP="$HOME/.omp/agent-config-backup-$(date +%Y%m%d-%H%M%S)"

mkdir -p "$BACKUP"
for file in config.yml mcp.json; do
  if [ -f "$DEST/$file" ]; then cp "$DEST/$file" "$BACKUP/"; fi
done
if [ -d "$DEST/extensions" ]; then
  mkdir -p "$BACKUP/extensions"
  cp -R "$DEST/extensions/." "$BACKUP/extensions/"
fi

mkdir -p "$DEST/extensions"
cp config.yml mcp.json "$DEST/"
cp -R extensions/. "$DEST/extensions/"
```

La copia de extensiones agrega o reemplaza archivos con el mismo nombre; no elimina otras extensiones que ya existan en el equipo.

### 3. Instalar los plugins

Ejecuta los comandos registrados en `plugins.json`:

```bash
omp plugin install @plannotator/pi-extension@0.25.1
omp plugin install @ramarivera/coding-buddy@0.9.4
omp plugin install plannotator@plannotator
```

Comprueba el resultado:

```bash
omp plugin list
```

### 4. Preparar los servidores MCP y variables de entorno

`mcp.json` declara estos servidores:

| Servidor | Requisito adicional |
|---|---|
| `obsidian` | `npx` y un vault de Obsidian que contenga la carpeta `OMP`. |
| `figma_desktop` | Figma Desktop con su MCP local disponible en `http://127.0.0.1:3845/mcp`. |
| `xcode` | macOS y `xcrun mcpbridge`. |
| `codegraph` | Ejecutable `codegraph` en `PATH`. |
| `sonarqube` | Ejecutable `sonar` y la autenticación de SonarQube configurada localmente. |
| `playwright` | Node.js/npm y acceso a `npx` para ejecutar `@playwright/mcp`. |
| `computer_use` | Opcional; requiere `open-computer-use` y está deshabilitado en esta configuración. |

La extensión `obsidian-memory.ts` requiere `OBSIDIAN_VAULT_PATH`. Define la ruta **al directorio padre** que contiene `OMP` (no a la carpeta `OMP` misma), por ejemplo:

```bash
export OBSIDIAN_VAULT_PATH="$HOME/Documents/Obsidian"
```

Configura esa variable en el perfil de tu shell, como `~/.zshrc`, si debe persistir entre terminales. Si el vault está en otra ubicación, usa su ruta real. Si no usas Obsidian, quita `extensions/obsidian-memory.ts` y el servidor `obsidian` de la configuración destino.

Los servidores MCP solo funcionarán si sus ejecutables y servicios están disponibles en el nuevo equipo. Instala las herramientas externas según sus instrucciones oficiales; las credenciales de SonarQube y otros servicios se configuran localmente, nunca en este repo.

### 5. Iniciar OMP y verificar

1. Cierra cualquier instancia de OMP abierta y vuelve a ejecutarla con `omp`.
2. Ejecuta `omp plugin list` y confirma las versiones indicadas en `plugins.json`.
3. Prueba solo los MCP para los que instalaste y configuraste los requisitos.
4. Inicia sesión en tu proveedor de modelos en la instalación destino; la autenticación no se exporta.

> [!WARNING]
> `config.yml` establece `tools.approvalMode: yolo`. Revisa esa opción y cámbiala antes de usar OMP si prefieres otro comportamiento de aprobación de herramientas.
