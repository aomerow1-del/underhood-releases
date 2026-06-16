# Underhood — Developer Guide

This document contains development, building, testing, and packaging workflows for Underhood. This information is intended for internal developers and should remain in the private repository.

---

## 📁 Repository Structure

```text
Underhood/
├── engine/               # Detection rules, environment audit, and dependency scan routines
│   ├── analyzer.py       
│   └── runner.py         # Self-repairing executor that catches errors and auto-installs packages
├── mcp-server/           # Standardized Model Context Protocol server exposing Underhood to AI agents
├── src-csharp/           # Windows desktop app source code in C# (WinForms)
├── src-macos/            # macOS native desktop app source code in Swift (SwiftUI)
├── vscode-extension/     # VS Code Extension source code, interface panels, and VSIX bundle
└── tests/                # Integration and runner self-repair validation suites
```

---

## 🛠 Building & Packaging

### Prerequisites
Make sure you have the following installed on your Windows machine:
1. **Python 3.10+** (with the `py` launcher)
2. **Node.js** (v18+)
3. **Visual Studio 2022** (with MSBuild and modern C# Roslyn compiler `csc.exe`)

### 1. Run Tests
Validate the analyzer rules and execution routines:
```powershell
# Run the quick analyzer test suite
py tests/test_analyzer_quick.py

# Run all unit and integration tests
py -m pytest tests/integration_test.py tests/test_runner_unit.py -v
```

### 2. Build the Desktop App & Installer
Run the integrated build script. This compiles the Python runner sidecar EXE, packages the obfuscated VS Code extension VSIX, compiles the C# WinForms application, and packages all final installer bundles:
```powershell
powershell -ExecutionPolicy Bypass -File .\build_all.ps1
```
*Compiled installer artifacts will be located under the `dist/` directory:*
- `dist\Underhood-Portable.zip` (Portable ZIP archive)
- `dist\UnderhoodInstaller.msi` (MSI Installer package)
- `dist\UnderhoodInstaller.exe` (Self-extracting EXE installer)

### 3. Package the VS Code Extension
To compile the TypeScript panels and package the `.vsix` bundle:
```powershell
cd vscode-extension
npm install
npm run package
```
*This outputs `vscode-extension/underhood-0.2.0.vsix`, which you can install directly into VS Code via "Install from VSIX...".*

### 4. Run the MCP Server
To run the Model Context Protocol server for AI integration:
```powershell
cd mcp-server
pip install .
python -m underhood_mcp
```
