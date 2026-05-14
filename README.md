# SI Projects

> Source Insight-style project management for C development in Visual Studio Code.

If you are coming from **Source Insight**, one of the things you miss most in VS Code is its project system — the ability to create a named project, tell it where your source lives, point it at your include directories (local or on a network share), save all of that, and reopen it later with a single click.

**SI Projects** brings that workflow to VS Code, and automatically configures IntelliSense so symbol lookup, Go-to-Definition, and `#include` suggestions work across your entire codebase — including VxWorks SDK headers, Linux sysroot headers, and Samba/UNC network paths.

---

## Features

### 📁 Project sidebar
All your saved projects are listed in a dedicated panel in the Activity Bar. Click any project to open it instantly. No file picker needed after the first time.

![Sidebar showing project list](images/sidebar.png)

### ✨ New Project wizard
A guided 4-step wizard collects everything needed to set up a project:
- Project name
- Working / source directory
- Include directories (optional, multi-entry, starts in your working dir)
- Additional directories (optional, e.g. vendor libs)
- C standard (`c89`, `c99`, `c11`, `c17`, `gnu11`, `gnu17`)

The wizard remembers your default `.siproj` save folder after the first run — you are never asked for it again unless you change it.

### 💾 .siproj project files
Each project is saved as a plain JSON `.siproj` file on disk. You can commit it to version control, share it with your team, or back it up like any other file.

### 🔌 Automatic IntelliSense configuration
Every time you open a project, SI Projects writes `.vscode/c_cpp_properties.json` into your working directory. This is what the [C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) reads for IntelliSense.

The configuration uses **compiler-free (tag-parser) mode** — no compiler needs to be installed or configured. This makes it work equally well for:

| Use case | Path type | Example |
|---|---|---|
| VxWorks / Windows SDK | Local Windows path | `C:\WindRiver\vxworks-7\include` |
| Linux cross-compilation | Samba / UNC network share | `\\192.168.1.10\nfs\sysroot\usr\include` |
| Embedded bare-metal | Local or network | `D:\SDK\arm\include` |

UNC network paths (`\\server\share\...`) are automatically converted to forward-slash format (`//server/share/...`) which cpptools accepts correctly.

### 📌 Persistent default projects folder
You choose once where `.siproj` files are saved. After that the wizard skips that step entirely. Change it any time with **SI Projects: Change Default Projects Folder** from the Command Palette.

### 🗑️ Project management
Right-click any project in the sidebar for:
- **Open Project** — open working directory and reload IntelliSense
- **Remove from List** — remove from sidebar, keep `.siproj` file on disk
- **Delete Project File** — remove from sidebar and permanently delete the `.siproj` file (source code is never touched)

### 🔁 Last-project memory
On startup, SI Projects detects the last opened project and offers to reopen it. If you are already in the correct working directory it stays silent.

---

## Requirements

- [C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) by Microsoft — provides the IntelliSense engine that SI Projects configures.
- No compiler required. SI Projects uses tag-parser mode which works without any toolchain installed.

---

## Getting Started

1. Install this extension and the C/C++ extension.
2. Open the **SI Projects** panel from the Activity Bar (window icon).
3. Click the **+** button or run **SI Projects: New Project** from the Command Palette (`Ctrl+Shift+P`).
4. Follow the wizard — enter a project name, pick your source directory, and add any include paths.
5. Your working directory opens automatically and IntelliSense is ready.

To reopen a project later, click it in the sidebar or run **SI Projects: Open Project**.

---

## Commands

| Command | Description |
|---|---|
| `SI Projects: New Project` | Run the new project wizard |
| `SI Projects: Open Project` | Open a `.siproj` file via file picker |
| `SI Projects: Close Project` | Clear the active project state |
| `SI Projects: Change Default Projects Folder` | Change where `.siproj` files are saved |

---

## The .siproj File

Projects are stored as human-readable JSON:

```json
{
  "name": "my_firmware",
  "version": "2.0",
  "workingDirectory": "C:/projects/my_firmware/src",
  "includeDirectories": [
    "//192.168.1.10/nfs/sysroot/usr/include",
    "C:/WindRiver/vxworks-7/include"
  ],
  "additionalDirectories": [],
  "cStandard": "c11",
  "createdAt": "2026-01-01T00:00:00.000Z",
  "lastOpenedAt": "2026-05-14T00:00:00.000Z"
}
```

You can edit this file manually if needed.

---

## After Opening a Project

If IntelliSense does not pick up includes immediately, run:

```
Ctrl+Shift+P → C/C++: Reset IntelliSense Database
```

This forces cpptools to re-index all headers. Usually only needed once after first setup or after adding new include directories.

---

## Known Limitations

- **Network share availability** — if a UNC include path is offline when VS Code starts, cpptools will silently skip those headers until the share is reachable again.
- **C only** — the wizard is designed for C projects. C++ files will open fine but the C standard selector only shows C standards.
- **One working directory per project** — matching Source Insight's model. If your project has multiple source roots, add them as additional directories.

---

## Contributing

Issues and pull requests are welcome at [github.com/TanvirAnzum/source_insight_project_creator](https://github.com/TanvirAnzum/source_insight_project_creator).

---

## License

MIT © [Tanvir Anzum](https://github.com/TanvirAnzum)
