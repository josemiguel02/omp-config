# Instrucciones para agentes

Este repositorio mantiene una exportación permitida de la configuración global de Oh My Pi (OMP). Para la guía completa orientada a usuarios, consulta [`README.md`](README.md).

## Contenido y fuente de verdad

- `config.yml`: preferencias globales de OMP. Revisa los cambios de seguridad antes de instalarlo; actualmente configura `tools.approvalMode: yolo`.
- `mcp.json`: definiciones de servidores MCP. Puede referenciar servicios locales y variables de entorno; no añadas credenciales.
- `extensions/`: código de extensiones versionado en este repo. Instala solo desde esta carpeta.
- `plugins.json`: inventario de plugins y versiones. Los paquetes se instalan desde sus registros; este repo no contiene su código instalado.
- `README.md`: pasos completos de instalación, dependencias y verificación.

Mantén esta lista como allowlist. No copies bases de datos, sesiones, cachés, blobs, directorios de autenticación, archivos `.env` ni el resto de `~/.omp/agent`.

## Instalación global

Cuando el usuario solicite instalar esta configuración en un equipo:

1. Confirma que `omp` está disponible y trabaja desde la raíz de este repositorio.
2. Avisa que `config.yml` usa `tools.approvalMode: yolo`; permite revisar o cambiar ese ajuste antes de reemplazar la configuración activa.
3. Respalda localmente los archivos existentes `config.yml`, `mcp.json` y `extensions/` bajo `~/.omp/agent/`. No agregues ese respaldo al repo.
4. Copia `config.yml` y `mcp.json` a `~/.omp/agent/`; copia el contenido de `extensions/` a `~/.omp/agent/extensions/`. No elimines otras extensiones del equipo sin autorización.
5. Instala los plugins globales registrados en `plugins.json` con sus comandos `omp plugin install`. No copies `node_modules` ni el caché de plugins.
6. Comprueba las dependencias de cada MCP antes de declararlo operativo. Por ejemplo, Obsidian requiere `OBSIDIAN_VAULT_PATH` apuntando al directorio padre de `OMP`; `xcode` requiere `xcrun mcpbridge`; los demás requisitos están documentados en `README.md`.
7. No exportes ni solicites credenciales para guardarlas aquí. La autenticación de proveedores y servicios se configura en el equipo destino mediante sus mecanismos seguros.
8. Verifica con `omp plugin list` y prueba solo los MCP cuyos ejecutables, variables y servicios estén disponibles.

Si el usuario no usa Obsidian, no actives `extensions/obsidian-memory.ts` ni el MCP `obsidian` sin configurar la ruta necesaria. No afirmes que un plugin o MCP está listo basándote solo en su definición o inventario.

## Cambios al inventario

Al añadir, quitar o actualizar plugins, compara `plugins.json` con `omp plugin list` en el equipo de origen. Registra nombres, versiones, alcance y comandos de instalación; excluye rutas locales y marcas de tiempo. Revisa todos los cambios para detectar secretos antes de confirmarlos o publicarlos. Mantén el repo privado.
