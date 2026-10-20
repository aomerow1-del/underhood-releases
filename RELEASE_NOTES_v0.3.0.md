# Underhood v0.3.0 Release Notes

**Release Date:** October 2026  
**Status:** Stable Release  
**Version:** `v0.3.0`  

---

## 🌟 Executive Overview

Underhood v0.3.0 marks the evolution of Underhood from a single/dual-runtime runner into an **Enterprise Multi-Runtime Self-Repairing Engine & Sandboxed Environment Platform**. 

With v0.3.0, vibe-coders, AI coding agents (Claude, Cursor, Windsurf, Devin), and developers can write and execute code across **10 programming runtimes** without worrying about environment initialization, manual installs, dependency confusion, or workspace pollution.

---

## 🚀 Key Highlights & New Capabilities

### 1. Multi-Runtime Self-Repairing Loop (10 Runtimes)
Underhood's proprietary error-interception engine now detects missing dependencies at runtime, parses stderr traces across all mainstream stacks, verifies package integrity, installs the missing packages into a local workspace sandbox, and re-executes the code smoothly.

| Runtime | Intercepted Error Patterns | Installer Mechanism | Sandbox Target |
|---|---|---|---|
| **Python** | `ModuleNotFoundError`, `No module named '...'` | `pip install --target .underhood_libs` | `.underhood_libs` |
| **Node.js** | `Cannot find module '...'` | `npm install / pnpm / yarn` | `node_modules` |
| **TypeScript** | `Cannot find module`, Bun/Deno/TS-Node errors | `bun add / npm install` | `node_modules` |
| **Go** | `no required module provides package ...` | `go get / go mod tidy` | `go.mod / go.sum` |
| **Rust** | `cannot find crate '...'` | `cargo add` | `Cargo.toml / target` |
| **.NET / C#** | `The type or namespace name '...' could not be found` | `dotnet add package` | `*.csproj` |
| **Ruby** | `cannot load such file -- ... (LoadError)` | `gem install --install-dir .underhood_gems` | `.underhood_gems` |
| **PHP** | `Class '...' not found`, `Failed opening required '...'` | `composer require` | `vendor` |
| **PowerShell** | `The term '...' is not recognized as the name of a cmdlet` | `Install-Module -Scope CurrentUser` | `.underhood_psmodules` |
| **Lua** | `module '...' not found` | `luarocks install --tree lua_modules` | `lua_modules` |

---

### 2. Anti-Dependency Confusion & Canonical Package Resolution
- **`is_local_module` Workspace Guardian:** Scans the active workspace before dispatching any package manager call. Prevents dependency confusion attacks where local filenames (e.g. `utils.py`, `config.py`, `helpers.ts`) are mistakenly installed from public registries (PyPI, npm).
- **Canonical Alias Map (`IMPORT_ALIAS_MAP`):** Real-world import names frequently differ from registry package names. Underhood maps imports directly to canonical package coordinates:
  - `PIL` → `Pillow`
  - `yaml` → `pyyaml`
  - `cv2` → `opencv-python`
  - `bs4` → `beautifulsoup4`
  - `sklearn` → `scikit-learn`
  - `dotenv` → `python-dotenv`
  - `jwt` → `PyJWT`
  - `dateutil` → `python-dateutil`
  - `git` → `GitPython`
  - `serial` → `pyserial`
  - And more than 40 common disambiguations.

---

### 3. Strict 4-Layer Sandboxing & Workspace Hygiene
Underhood isolates installed dependencies on a per-project basis, preventing global environment contamination:
- **Python:** `.underhood_libs/` injected via prepended `PYTHONPATH`.
- **Lua:** `lua_modules/` injected via `LUA_PATH` and `LUA_CPATH`.
- **Ruby:** `.underhood_gems/` isolated using `GEM_HOME` and `GEM_PATH`.
- **PowerShell:** `.underhood_psmodules/` isolated using scoped `PSModulePath`.
- **1-Click Sandbox Purge:**
  - CLI: `python -m engine.runner --clean-sandbox`
  - VS Code Command Palette: `Underhood: Clean Workspace Sandbox`
  - MCP Tool: `clean_sandbox`

---

### 4. Dependency Pinning & Manifest Exporter
- **Reproducible Lockfile (`.underhood/underhood.lock.json`):** Tracks every auto-repaired package, runtime origin, installation timestamp, version pinned, and checksum verification status.
- **Manifest Generation (`--export-manifest`):**
  - Effortlessly convert vibe-coded experiments into production-ready requirements:
    - Python → generates `requirements.txt`
    - Node / TypeScript → updates `package.json`
    - Ruby → generates `Gemfile`
    - PHP → updates `composer.json`
  - Accessible via CLI (`--export-manifest`), VS Code (`Underhood: Export Discovered Dependencies`), and MCP Server (`export_manifest`).

---

### 5. VS Code Extension & MCP Server v0.3.0
- **VS Code Extension v0.3.0 (`underhood-0.3.0.vsix`):**
  - Upgraded command palette entries for `cleanSandbox` and `exportManifest`.
  - Live sandbox status visual indicators in the webview panel.
  - Multi-file extension triggers for `.py`, `.js`, `.ts`, `.tsx`, `.lua`, `.go`, `.rs`, `.cs`, `.rb`, `.php`, `.ps1`.
- **MCP Server Tools:**
  - `analyze_project`: Deep static project and missing-tool analysis for AI agents.
  - `run_code_with_repair`: Execute and auto-repair code safely with live JSON output.
  - `clean_sandbox`: Remove sandboxed dependencies and lockfiles.
  - `export_manifest`: Consolidate discovered sandbox dependencies into official manifests.

---

### 6. Security & Credential Scanning Audit
- **Zero Secrets / Leaks:** Verified via automated secrets scan and repository inspection.
  - 0 Critical secrets.
  - 0 High-risk credential leaks.
  - 0 Hardcoded tokens, API keys, or private keys.
- **Strict Input Sanitization:**
  - Package names extracted from stack traces are vetted against strict shell metacharacter filters (`&`, `;`, `|`, `$`, `` ` ``, `>`, `<`, newlines, directory traversals).

---

### 7. CI / CD Hardening & Platform Support
- **Cross-Platform Compilation:** Fixed Roslyn C# compiler discovery on Visual Studio Enterprise (GitHub Actions Windows runners) and BuildTools installations.
- **macOS VSIX Automated Builds:** Pre-compiles TypeScript entrypoints before packaging VSIX artifacts in GitHub Actions.
- **Test Suite:** 64 comprehensive pytest unit and integration tests passing at 100% pass rate.

---

## 📦 Distribution Artifacts
- `dist/UnderhoodInstaller.msi` — Native Windows Desktop Installer
- `dist/UnderhoodInstaller.exe` — Self-contained Windows Executable Installer
- `dist/Underhood-Portable.zip` — Windows Portable Package
- `dist/Underhood-macOS.zip` — macOS Universal SwiftUI Bundle
- `vscode-extension/underhood-0.3.0.vsix` — VS Code Extension v0.3.0
- `mcp-server/` — Model Context Protocol Server for Claude, Cursor, Windsurf
