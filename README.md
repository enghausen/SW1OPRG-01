# 🚀 VS Code & C/C++ Development Guide

This document serves as a personal guide for the **SW1OPRG-01** course at **Aarhus University**, aimed at **learning C/C++ development using VS Code**. It compiles best practices and setup instructions for **VS Code**, including installation, debugging, compilation, and extensions.

---

## 📦 Installing MSYS2 for C++

1. Download **MSYS2** from [msys2.org](https://www.msys2.org/).
2. Install MSYS2 and open the terminal (**MSYS2 UCRT64** is recommended for C++ development).
3. Update packages with:
   ```sh
   pacman -Syu
   ```
4. Install essential development tools:
   ```sh
   pacman -S --needed base-devel mingw-w64-ucrt-x86_64-toolchain
   ```
5. Verify installation:
   ```sh
   gcc --version
   g++ --version
   gdb --version
   ```

---

## 🛠 Setting Up VS Code for C++

1. Install **VS Code** from [code.visualstudio.com](https://code.visualstudio.com/).
2. Install the following **extensions**:
   - **C/C++** (`ms-vscode.cpptools`)
   - **Markdown All in One** (`yzhang.markdown-all-in-one`) (For improved Markdown preview)
   - **LaTeX Workshop** (`James-Yu.latex-workshop`) (For rendering mathematical formulas in Markdown)
   - **Doxygen Documentation Generator** (`cschlosser.doxdocgen`) (For automatic header comments)
3. Set up **MSYS2** as the default terminal:
   - Open `settings.json` (Ctrl+Shift+P → *Preferences: Open Settings (JSON)*)
   - Add:
     ```json
     "terminal.integrated.profiles.windows": {
         "MSYS2": {
             "path": "C:/msys64/usr/bin/bash.exe"
         }
     },
     "terminal.integrated.defaultProfile.windows": "MSYS2"
     ```
4. Configure `tasks.json` and `launch.json` for automated building and debugging.

---

## ⚙️ Setting Up VS Code Debugging

### 🏗 Configuring `tasks.json`

This file automates compilation.

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "Build MyProject",
            "command": "C:/msys64/ucrt64/bin/g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${workspaceFolder}/src/main.cpp",
                "${workspaceFolder}/src/utils.cpp",
                "-o",
                "${workspaceFolder}/build/my_project.exe"
            ],
            "options": {
                "cwd": "${workspaceFolder}/build"
            },
            "problemMatcher": ["$gcc"],
            "group": {
                "kind": "build"
            },
            "detail": "Compiles MyProject."
        }
    ]
}
```

### 🐞 Configuring `launch.json`

This enables debugging.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Run MyProject",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/my_project.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "preLaunchTask": "Build MyProject"
        }
    ]
}
```

This ensures **MyProject** is built **before debugging starts**.

---

## 🏗 Manual Compilation and Linking

If you prefer manual compilation in the terminal, use the following commands:

### 🔹 Compile a single C++ file:

```sh
g++ -o program main.cpp
```

### 🔹 Compile multiple files:

```sh
g++ -c utils.cpp -o utils.o
g++ -c main.cpp -o main.o
g++ utils.o main.o -o my_project.exe
```

### 🔹 Compile only the modified file:

If you only changed `utils.cpp`, you can save time by compiling just that file:

```sh
g++ -c utils.cpp -o utils.o
g++ utils.o main.o -o my_project.exe
```

---

## ✍️ Generating Documentation in VS Code

VS Code can **automatically generate function comments**. To do this:

1. Write the function signature in a `.h` file (e.g., `utils.h`).
2. Place the cursor over the function, type `/**`, and press **Enter**.
3. VS Code generates a comment block like this:
   ```cpp
   /**
    * Calculates the volume of a sphere.
    * @param radius The radius of the sphere.
    * @return The volume of the sphere.
    */
   ```

To enable this, install the **Doxygen Documentation Generator** extension.

---

## 🏗 Understanding `preLaunchTask`

### 🔹 What is `preLaunchTask`?

`preLaunchTask` in `launch.json` ensures that the project is built **before debugging starts**.

### 🔹 Example in `launch.json`:

```json
"preLaunchTask": "Build MyProject"
```

This means **VS Code will automatically build the project** before starting the debugging session.

### 🔹 Matching `preLaunchTask` with `tasks.json`

In `tasks.json`, the label must match **exactly**:

```json
"label": "Build MyProject"
```

This ensures a **cohesive workflow** between building and debugging.

---

## ✅ Summary

- **Installed MSYS2** and configured the development environment.
- **Configured VS Code** with terminal settings and necessary extensions.
- **Set up `tasks.json` and `launch.json`** for automated compilation and debugging.
- **Learned manual command-line compilation** for multiple files.
- **Enabled automatic function documentation** in header files.
- **Understood `preLaunchTask`** and how it integrates with `tasks.json`.

This document will be continuously updated with more tips and best practices! 🚀