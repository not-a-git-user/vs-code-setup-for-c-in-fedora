# vs-code-setup-for-c-in-fedora

First, install VS Code:

```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf install code
```

Install required C development tools:
```
sudo dnf4 groupinstall "Development Tools"
sudo dnf install gcc gcc-c++ gdb make cmake
```




Open VS Code and install these essential extensions:
- C/C++ (by Microsoft)
- C/C++ Extension Pack
- Code Runner (optional but useful)
- CMake Tools (if you plan to use CMake)


Create a basic C project structure:
```
mkdir c_project
cd c_project
code .
```

Create these two files in your projectfolder and add these configuration files:
`tasks.json` for compilation:
```
jsonCopy{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "C: gcc build active file",
            "type": "shell",
            "command": "gcc",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "options": {
                "cwd": "${workspaceFolder}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
`launch.json` for debugging:
```
jsonCopy{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "gcc - Build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C: gcc build active file",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
```

Test your setup with a simple C program:

```
#include <stdio.h>
int main() {
    printf("Hello, World!\n");
    return 0;
}
```
You can now:
Build: Press Ctrl+Shift+B
Debug: Press F5
Run: Right-click and select "Run Code" (if using Code Runner)
