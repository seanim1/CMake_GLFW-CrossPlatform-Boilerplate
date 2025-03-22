# CMake GLFW skeleton for Windows, Linux, MacOS

## Setup Instructions for Windows

using vcpkg to download glfw

### Step 1: Install vcpkg

First, you need to install `vcpkg`, a C++ package manager.

1. Open **Command Prompt** or **PowerShell**.
2. Clone the `vcpkg` repository in your C drive root:

   ```bash
   cd \ # Navigate to the root of the C:\ drive
   git clone https://github.com/microsoft/vcpkg.git
3. Navigate to the `vcpkg` directory and execute the bootstrap script:

   ```bash
   cd vcpkg
   .\bootstrap-vcpkg.bat

### Step 2: Install glfw

Now install the GLFW3.

1. Clone the `vcpkg` repository in your C drive root:

   ```bash
   ./vcpkg install glfw3:x64-windows

2. Now you should be able to locate .cmake files at "C:\vcpkg\installed\x64-windows\share\glfw3" and "glfw3.dll" at "C:\vcpkg\installed\x64-windows\bin"
