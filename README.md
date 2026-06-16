# Underhood — Zero-Effort Project Runner & Environment Analyzer

Underhood is a sophisticated local analyzer and runner built for developers, vibe-coders, and AI agents. It automatically detects project types, audits local environment toolchains, highlights missing SDKs with 1-click terminal install commands (or manual download fallbacks), and executes runtimes with a self-repairing loop that catches missing dependencies and auto-installs them on the fly.

> [!NOTE]
> **Proprietary Software Notice:** Underhood is proprietary, closed-source software. This repository hosts our issue tracker, user documentation, and release installers. Compilations and source code are kept private.

---

## 🗺️ Interactive System Architecture

To understand the core design, visual workflows, and safety routines of Project Underhood, we have created an interactive blueprint dashboard. Open it directly in your web browser:
👉 **[underhood_architecture.html](../Underhood/underhood_architecture.html)**

---

## 🚀 Key Features

* **Universal Layout Detection (16+ Types):** Recognizes project types, project name, and version from manifest files. Supports nested layout structures up to depth 1 (e.g. game projects nested inside subdirectory repositories).
* **Local Tool Audit Grid:** Rapidly verifies presence and version of compilers, runtimes, package managers, and SDKs (Node, Python, Java JDK, Android SDK, Gradle, Rust, Unity, Godot, Go, CMake, and more).
* **Dynamic Package Manager Fallbacks:** If a required tool is missing, Underhood intelligently generates installation commands by checking for `winget`, and gracefully falls back to `choco` or `scoop`. If no package managers exist, it provides official manual download links.
* **Self-Repairing Execution Loop:** Executes Python, Node.js, and Lua scripts. If it encounters a missing dependency (e.g., `ModuleNotFoundError` or `Cannot find module`), it traps the error, resolves the package name, installs it locally via `pip/npm/luarocks`, and retries execution.
* **Triple Interface Architecture:** Use it as a drop-zone **Tauri Desktop Application**, a fully integrated **VS Code Extension**, or a native **MCP Server** for AI coding assistants.

---

## 💻 The Three Interfaces

### 1. VS Code Extension
The Underhood VS Code extension brings the power of the engine directly into your editor's sidebar.
- **Drag-and-Drop:** Simply drag any folder from your OS or VS Code Explorer directly into the Underhood sidebar to instantly analyze the project requirements.
- **Rich Missing Tools UI:** Parses structured JSON output from the engine and generates a beautiful, actionable UI for missing SDKs.
- **1-Click Installs:** Missing a tool? Click the "Run Install Command in Terminal" button to automatically open a VS Code terminal and paste the exact `winget`/`choco`/`scoop` command required to install it.

### 2. MCP Server (Model Context Protocol)
Underhood includes a built-in MCP server, allowing AI coding assistants like **Claude Desktop** and **Cursor** to natively invoke Underhood as a tool.
- **Zero-Config Agent Troubleshooting:** Agents can invoke the `analyze_project` tool to understand exactly what a repository is, what tools it requires, and what is missing on your local machine, allowing them to fix environment errors autonomously without guessing.

### 3. Tauri Desktop GUI
A standalone, sandboxed desktop application where you can drag-and-drop project folders. It visualizes the execution pipeline, showing exactly which dependencies were auto-installed in the sandboxed `.underhood_libs` directory before running your code.

---

## 📥 Download & Installation

Underhood is distributed as pre-compiled installers and extensions. To download and install the software, please refer to the **Releases** page on GitHub:

### 1. Tauri Standalone Desktop Application (Windows & macOS)
* Navigate to the latest release on our GitHub Releases page.
* Download the appropriate installer:
  * **Windows**: `Underhood-Setup.msi` or standalone `Underhood.exe`
  * **macOS**: `Underhood.dmg`
* Run the installer file and complete the installation wizard setup.

### 2. VS Code Extension
* Download the packaged extension file (`underhood-0.1.0.vsix`) from the latest release page.
* Open VS Code, open the Extensions side panel (`Ctrl+Shift+X` or `Cmd+Shift+X`).
* Click the menu icon (`...`) in the top-right corner of the Extensions view and select **Install from VSIX...**.
* Select the downloaded `.vsix` file to install it directly.

---

## 🛠 Supported Project Layouts & Runtimes

Underhood automatically detects and audits the following project structures:

| Project Type | Marker File(s) Detected | Audited Tools & Runtimes |
|---|---|---|
| **Godot Engine** | `project.godot` | Godot Engine executable |
| **Unity** | `ProjectSettings/`, `Assets/` | Unity Hub, Unity Editor, Android target SDKs |
| **Android** | `AndroidManifest.xml`, Gradle files | Java JDK 11+, Android SDK, Android Studio, Gradle |
| **Flutter** | `pubspec.yaml` | Flutter SDK, Dart SDK, Java JDK, Android SDK |
| **Rust** | `Cargo.toml` | Cargo, Rust compiler (`rustc`), MSVC C++ Linker |
| **Go** | `go.mod` | Go SDK |
| **Java Maven** | `pom.xml` | Java JDK 11+, Apache Maven (`mvn`) |
| **Java Gradle** | `build.gradle`, `build.gradle.kts` | Java JDK 11+, Gradle builder |
| **.NET / C#** | `*.sln`, `*.csproj` | .NET SDK, MSBuild |
| **C++ / CMake** | `CMakeLists.txt` | CMake compiler toolchain |
| **C/C++ Make** | `Makefile`, `makefile` | Make / MinGW compilers |
| **PHP** | `composer.json` | PHP interpreter, Composer |
| **Ruby** | `Gemfile` | Ruby interpreter, Bundler |
| **Node.js** | `package.json` | Node.js, npm package manager |
| **Python** | `requirements.txt`, `pyproject.toml`, `setup.py` | Python interpreter, pip package manager |
| **Lua** | `*.rockspec`, `*.lua` | Lua interpreter, Luarocks package manager |

---

## 🦾 How to Integrate Underhood with AI Coding Agents (Token Savings Guide)

Adding Underhood to your AI coding agents is the most effective way to eliminate multi-turn compile/runtime debug sessions, saving you **thousands of tokens** per development run.

### 1. Via MCP Server (Recommended)
Add the Underhood MCP server to your Claude Desktop or Cursor configuration. The agent will automatically discover the `analyze_project` tool and use it to debug local environment issues autonomously.

### 2. Via System Prompt / `.cursorrules`
If you are using the CLI directly, inject the following instruction block into the system prompt:
```text
Role Instruction: Environment Execution & Troubleshooting
You are equipped with a local Windows Environment Resolver tool called "Underhood". 
If the user's project lacks packages or compilers, do not engage in multi-turn troubleshooting. Run:
`underhood <path_to_project_root_or_file>`
This runs the file with a self-repairing loop that catches missing dependencies and resolves them locally. Only report back to the user when execution completes successfully or outputs logical tracebacks.
```

### Why This Saves Money (Token Math):
Without Underhood, resolving a 3-package script setup takes multiple LLM prompts:
$$\text{Prompt 1 (Run)} \rightarrow \text{Error A} \rightarrow \text{Prompt 2 (Fix A)} \rightarrow \text{Error B} \rightarrow \text{Prompt 3 (Fix B)} \rightarrow \dots$$
Each step sends your entire source code context and history back to the LLM API. 
* **Without Underhood:** 3 round-trips $\approx$ **15,000 to 30,000 tokens** consumed.
* **With Underhood:** 1 round-trip $\approx$ **1,500 tokens** (Underhood repairs the missing packages locally in one execution, returning the finished output).

---

## 📝 Logging & Diagnostics

Underhood maintains detailed diagnostic logs for troubleshooting environment issues and tracking executions.

### Log File Locations

* **Tauri Desktop Application (Windows):** `%APPDATA%\com.underhood.dev\logs\underhood.log`
* **Tauri Desktop Application (macOS):** `~/Library/Logs/com.underhood.dev/underhood.log`
* **VS Code Extension (VSIX):** `~/.underhood/logs/underhood-YYYY-MM-DD.log` (daily rotating)

### VS Code Extension Output Channel
In addition to the daily log files, you can view real-time log output directly in VS Code:
1. Open the **Output** panel (`Ctrl+Shift+U` or `Cmd+Shift+U`).
2. Select **Underhood** from the dropdown menu in the top-right corner.

### VS Code Configuration Options
You can customize the logging verbosity for the VS Code extension in your `settings.json`:
* **`underhood.logLevel`**: Controls the level of logs recorded. Options: `debug`, `info`, `warn`, `error`, `off` (Default: `info`).

### Log Rotation Policy
* **Desktop App:** Automatically rolls over when the log file reaches **5 MB**, retaining a maximum of **3 backups** (`underhood.log.1`, `underhood.log.2`, etc.).
* **VS Code Extension:** Automatically rotates daily into date-stamped files (`underhood-YYYY-MM-DD.log`).

### Log Format
All logs follow a structured prefix:
```text
[2026-05-31 23:44:28 INFO ] [underhood] message here
```

---

## 🛡️ VirusTotal Malware Scanning

Underhood includes a built-in security feature to scan executable and script files (`.exe`, `.dll`, `.so`, `.py`, `.js`) against the VirusTotal database before running them.

**Privacy First:** This is a **hash-only** scan. Underhood calculates the SHA-256 hash of each file locally and only sends the hash to the VirusTotal API. Your actual source code and file contents are **never uploaded**.

### How to Enable
* **Tauri Desktop App:** Go to the **Scanner Settings** tab in the main interface, toggle it on, and enter your VirusTotal API key.
* **VS Code Extension:** Open VS Code settings, search for `underhood.virusTotalEnabled` and check the box. Then, enter your API key in `underhood.virusTotalApiKey`.

If threats are detected, the scan will immediately block execution and surface a warning banner in the UI showing which files were flagged.

---

## ⚖️ License & Terms

Underhood is proprietary software. By installing, copying, or otherwise using the Software, you agree to the terms of the [Underhood End User License Agreement (EULA)](License.rtf). 

Unauthorized distribution, copying, modification, or reverse engineering of the binary files, extension files, or installer packages is strictly prohibited.

---

## 💻 Development Guide

If you are working on the internal development or building Underhood from source, please refer to the private developer guide:
👉 **[DEVELOPER.md](DEVELOPER.md)**
