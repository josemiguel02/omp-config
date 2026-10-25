# OMP config

Global Oh My Pi configuration export. Contains `config.yml`, `mcp.json`, custom extensions, and a versioned inventory of plugins reported by `omp plugin list`.

## Restore

Copy `config.yml`, `mcp.json`, and `extensions/` into `~/.omp/agent/`. Reinstall plugins from the commands in [`plugins.json`](plugins.json), then check with `omp plugin list`.

Install the required MCP executables separately. Recreate environment variables used by MCP definitions (for example, `OBSIDIAN_VAULT_PATH`) on the target machine; secrets and credentials are intentionally not stored here.

## Security

This repository excludes OMP databases, sessions, caches, credentials, and installed plugin package files. Keep the GitHub repository **private**: configuration and extension source can still reveal personal setup details. Review changes before pushing.
