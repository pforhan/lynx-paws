# lynx-paws
Portable Atari Workshop Suite for the Lynx

> note: ai-generated readme below.  Sources: 
> * https://forums.atariage.com/topic/315877-cc65-development-in-visual-studio-code/
> * https://atarilynxdev.net/doku.php/start
> * https://www.github.com/AtariLynx/devcontainers
> * random searches

## Overview

This project provides a **robust and isolated, cross-platform development environment** for Atari Lynx programming using Docker and Visual Studio Code. It aims to simplify the historical challenges of setting up Lynx development tools, which often were limited to Windows or involved complex configurations. By leveraging Docker, you can ensure a **clean and reproducible toolchain** that doesn't "pollute your host OS".

## 🚀 Key Features

*   **Cross-Platform Compatibility**: Develop Lynx software on Linux, macOS, or Windows. The Lynx toolkit (BLL, newcc65, CC65) supports Linux and macOS, unlike older Visual Studio versions that were limited to Windows.
*   **Easy Environment Setup**: Remote Containers in VS Code streamline the setup process on any new machine.
*   **Isolation and Cleanliness**: All development tools are installed within the Docker container, keeping your host operating system clean and uncluttered. This provides an "isolated environment".
*   **Reproducibility**: The Dockerfile serves as a description to build an image. This image acts as a template for a container, allowing for a "frozen toolchain that can be reproduced many years later". The `.devcontainer` folder, including the Dockerfile, can be part of your Git repository, ensuring that collaborators can rebuild the exact toolset you have used with specific versions. Alternatively, by getting the "latest sources of the tooling," it provides an "always up-to-date toolchain when the image is rebuilt".
*   **Lightweight and Flexible**: Containers are "very lightweight when compared with VM images". They offer "software virtualization" rather than hardware virtualization like VMs. This means you can "run multiple containers with different versions of the toolchain side-by-side" without reinstallation conflicts.

## 🛠️ Key Development Tools Included in this Image

The `Dockerfile` in this setup includes a comprehensive array of tools essential for Atari Lynx development, primarily focusing on C and Assembly language programming.

*   **CC65**: A C compiler specifically designed for 6502-based systems like the Atari Lynx. The image builds CC65 from the `atarilynx/lynx` Bitbucket repository's tools folder.
*   **lyxass**: The Atari Lynx assembler, confirmed to work well on 64-bit platforms and includes 64-bit math support (needed for Jaguar development, which uses a 64-bit architecture).
*   **BLL (new\_bll)**: Bastian's Lynx Library, a crucial library for Lynx development.
*   **lynxenc / make\_lnx**: Tools for encryption/decryption and creating executable `.lnx` files for the Lynx, included as part of the `new_bll` repository.
*   **sprpck**: A utility for packing sprites, streamlining graphical asset integration.
*   **hmcc**: A helper tool useful for cross-compilation processes.
*   **tp**: A tool related to Tiny Pascal compilation.
*   **pretty6502**: A formatter for 6502 assembly code, aiding in code readability.
*   **nawk (awk)**: A powerful text processing utility, part of the base system packages.

## ⚡ Prerequisites

Before you begin, ensure you have the following installed on your **host operating system**:

*   **Visual Studio Code**: Download and install the IDE from [https://code.visualstudio.com/download](https://code.visualstudio.com/download).
*   **Docker**: Set up Docker Desktop for container support. Follow the instructions at [https://code.visualstudio.com/docs/remote/containers](https://code.visualstudio.com/docs/remote/containers).
    *   **Windows Specific**: If you are running Windows 10 Home, Docker requires Hyper-V or WSL2. For Windows 10 Professional, Hyper-V should typically not be an issue.

## 🚀 Getting Started

Follow these steps to set up and use the development container:

1.  **Clone this Repository**:
    ```bash
    git clone [your-repository-url]
    cd [your-repository-name]
    ```
2.  **Open Project in VS Code**: Open the cloned folder in Visual Studio Code.
3.  **Add Dev Container Configuration Files**:
    *   Open the **Command Palette** in VS Code (F1 or Ctrl+Shift+P).
    *   Type and select `Remote-Containers: Add Development Container Configuration Files...`.
    *   You can choose a base image that closely matches `debian:stable-slim` (e.g., "Debian" or "Alpine") when prompted. This will generate a default `devcontainer.json` and `Dockerfile` in a new `.devcontainer` folder.
4.  **Replace Dockerfile Content**: **Replace the content of the newly generated `Dockerfile`** in the `.devcontainer` folder with the comprehensive `Dockerfile` provided in this repository (e.g., `lynxdev-Dockerfile.txt`).
5.  **Reopen in Container**:
    *   Open the **Command Palette** again (F1 or Ctrl+Shift+P).
    *   Select `Remote-Containers: Reopen in Container`.
    *   VS Code will now build the Docker image (this may take some time during the initial setup) and then start a container instance.
    *   You should see the bottom of your VS Code screen change, indicating you are connected to the container (e.g., displaying "Debian" or a custom name you've set in `devcontainer.json`).
6.  **Verify Tools**: Open the Terminal window in VS Code (Ctrl+` or View > Terminal). You should now be inside the root of your source code folder, but within the container. All the installed CC65 and Lynx tools will be available for use. Trying to use these tools outside the container would result in an error unless separately installed on your host OS.

## ⚙️ Configuring Build Tasks

To easily compile your Atari Lynx projects within the container, you can set up a `tasks.json` file:

1.  **Configure Tasks**: Press **Ctrl+Shift+B** (or go to `Terminal > Configure Tasks` from the top menu).
2.  **Create `tasks.json`**: Choose to create a `tasks.json` file. This file will be located under the `.vscode` folder in your project root.
3.  **Add Build Task**: Replace the content of `tasks.json` with a task that executes your build command (assuming you have a `Makefile` in your project root):

    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "label": "Build Lynx Project",
                "type": "process",
                "command": "make",
                "args": [
                    "all"
                ],
                "problemMatcher": [],
                "group": {
                    "kind": "build",
                    "isDefault": true
                }
            }
        ]
    }
    ```
    This task will execute the `make all` command when you press Ctrl+Shift+B, utilizing your project's `Makefile`. You can add additional tasks for `clean` or `install` as needed.

## 🎮 Running and Emulating Your Lynx Projects

**Important Note**: The **development container does NOT include a Lynx emulator**. The setup's primary purpose is to provide the necessary toolchain for **building** Atari Lynx ROMs (files ending in `.lnx`).

Running a graphical emulator with a user interface directly within a Docker container and displaying it on your host machine's graphical interface typically requires additional configurations like X11 forwarding or VNC servers, which **are not set up in this dev container**.

The recommended workflow is to **compile your Atari Lynx projects within the dev container**, and then transfer or access the resulting `.lnx` ROM files on your **host operating system** to run them using a separate Lynx emulator installed directly on your machine.

### Popular Atari Lynx Emulators for your Host OS:

Most Lynx emulators are based on the original Handy Lynx Emulator. To play games, emulators typically require ROM files (dumps of cartridge data) and sometimes the Lynx System BIOS ROM file.

*   **RetroArch**: Highly recommended open-source multi-system emulator available for most platforms (macOS, Linux, Windows). It uses "cores" (digital recreation of hardware chips and wires) for emulation, including a Libretro core for Lynx. It is simple to use and a trusty program.
*   **MAME (Multiple Arcade Machine Emulator)**: An open-source emulator known for recreating arcade machines but also capable of emulating many other old systems, including the Lynx. It is simple to use and provides great results. MAME was designed primarily as a research tool to perfectly recreate arcade machines.
*   **OpenEmu**: An open-source emulator particularly popular for macOS users, known for its slick interface and ROM management capabilities. It uses cores like RetroArch, such as Mednafen, for Lynx emulation. It is described as the "iTunes of emulation" with easy-to-navigate menus and simple controller setup. It also supports Windows and Android.
*   **Handy**: The original open-source Lynx emulator, available for Windows and macOS, and often used as a base for other emulators. It is described as the simplest way to run Atari Lynx ROMs on your computer.
*   **Holani**: A newer open-source emulator project written in Rust, available for Linux, macOS, and Windows, also offering a LibRetro core.
*   **Felix**: An early-stage emulator project for Windows with enhanced features like tracing.
*   **Argon**: A closed app primarily for Android handheld devices, focusing on Atari consoles including the Lynx (2600 to Lynx and NES). It is available from the Google Play Store.

## ⚠️ Important Considerations & Known Issues

*   **Toolchain Versioning**: The current `Dockerfile` directly clones and compiles the "latest sources of the tooling," providing an "always up-to-date toolchain when the image is rebuilt". If you require a "frozen toolset" (e.g., a specific version of CC65 like CC65:2.19, or newcc65:feb2021), a Docker image could be created and shared on a public registry like Docker Hub.
*   **Newcc65 Compilation Issues**: During initial discussions, LX.NET encountered errors when trying to compile `newcc65` from Karri's `atarilynx` repository on Ubuntu, including "unknown type name ‘intptr_t’" and "lvalue required as left operand of assignment", along with several cast warnings. Karri acknowledged these as potentially familiar issues that might have been fixed previously. The provided `lynxdev-Dockerfile.txt` uses the Bitbucket `atarilynx/lynx` repository for CC65, which might bypass these specific issues if that version is more stable.
*   **Lynx Encryption Tools**: The compilation of `lynxenc` and `lynxdec` tooling from the `lynx-encryption-tools` repository had pending pull requests for fixes. The provided `lynxdev-Dockerfile.txt` includes `lynxenc` from `new_bll/lynxenc` which indicates it is expected to work.
*   **Environment Variables**: LX.NET noted that `CC65INCLUDE` and `CC65LIB` environment variables were not initially set. The `lynxdev-Dockerfile.txt` explicitly sets crucial environment variables like `CC65_HOME`, `CA65_INC`, `CC65_INC`, `LD65_CFG`, `LD65_LIB`, and `LD65_OBJ` to ensure the build tools correctly locate their necessary files.

## 🤝 Contribute

This project is built on the spirit of community and collaboration for Atari Lynx development. Feedback and improvements are welcome.

## Addendum: Displaying the Emulator UI from Docker on the host

To show the emulator’s graphical interface (like Handy or Mednafen), you’ll need to forward the display from the container to your host system. Common methods include:

* X11 forwarding (Linux/macOS with XQuartz or native X server)
  ```bash
  xhost +local:docker
  docker run -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix ...
* VNC or web-based UI (some emulators support this directly)
* Wayland support (experimental and less common)

On Linux, this is relatively smooth. On macOS or Windows, you’ll need extra tools like XQuartz (macOS) or an X server like VcXsrv (Windows).