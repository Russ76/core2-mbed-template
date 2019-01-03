# core2-mbed-template
Blinking leds project template for [Husarion CORE2 board](https://husarion.com/manuals/core2/) equipped with STMF407ZG MCU. 
Project is preapared for development in Visual Studio Code with no additional extensions except Mbed CLI tools.

<p align="center"><img width="800px" src="https://cdn.shopify.com/s/files/1/2545/8446/products/CORE2-ROS_1024x1024@2x.png?v=1520001976" alt="CORE2-ROS"/></p>

## Important
This target use C++11 and is available for `GCC_ARM` toolchain only!

## Prerequisites
You need following tools:
* [Visual Studio Code IDE](https://code.visualstudio.com/)
* [GNU Arm Embedded version 6 toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
* [STM32 ST-LINK Utility](https://www.st.com/en/development-tools/stsw-link004.html) (Windows only)
* [stlink flasher](https://github.com/texane/stlink/blob/master/README.md) (Mac/Linux/Windows)
## mbed-cli installation
In-depth tutorial on Mbed CLI tool installation can be found [here](https://os.mbed.com/docs/v5.10/tools/installation-and-setup.html). 

**Important!**

After installation Mbed CLI needs to know the path to the binary directory of your **GCC Arm Embedded Compiler** installation. You can read how to do it under this [link](https://os.mbed.com/docs/v5.10/tools/configuring-mbed-cli.html#through-mbed-cli-configuration). You will also find all necessary information about this tool there.

Example for Windows:
```bash
$ where arm-none-eabi-gcc # prints path to arm-none-eabi-gcc.exe if in PATH
$ mbed config -G GCC_ARM_PATH "C:\Program Files (x86)\GNU Tools ARM Embedded\6 2017-q2-update\bin" # configure path for mbed-cli
$ mbed config --list # check configuration
```

### Preparing a workspace
Create a new folder `core2-mbed-workspace`. It will serve as workspace for your mbed projects.  Run:

```bash
$ mkdir core2-mbed-workspace && cd core2-mbed-workspace
```
Next step is to import `mbed-os` library. It will be used by all your projects. In your workspace folder run:

```bash
$ mbed import mbed-os
[mbed] Working path "E:\mbed_projects\core2-mbed-workspace" (directory)
[mbed] Program path "E:\mbed_projects\core2-mbed-workspace"
[mbed] Importing program "mbed-os" from "https://github.com/ARMmbed/mbed-os" at latest revision in the current branch
```

After that you will need to set path to `mbed-os` directory in Mbed CLI. These way all your projects can use one instance of library (default configuration is to have separate instance of library for each project). Run:

```bash
$ mbed config -G MBED_OS_DIR <path to mbed-os>
```

Example:

```bash
$ mbed config -G MBED_OS_DIR "E:\mbed_projects\core2-mbed-workspace\mbed-os"
[mbed] Working path "E:\mbed_projects\core2-mbed-workspace" (directory)
[mbed] Program path "E:\mbed_projects\core2-mbed-workspace"
[mbed] E:\mbed_projects\core2-mbed-workspace\mbed-os now set as global MBED_OS_DIR
```

### Importing template project

Open Visual Studio Code, press `CTRL + SHIFT + P` and type `Git: Clone` in Command Pallet. Copy and paste `https://github.com/byq77/core2-mbed-template.git` URL.

<p align="center"><img width="800px" src="https://user-images.githubusercontent.com/28567623/48676115-53336680-eb62-11e8-9059-4aeb8d8096b6.png" /></p>

You will be prompted to select your repo location. Choose `core2-mbed-workspace` folder. Open your repo in vs code. You should see:

<p align="center"><img width="800px" src="https://user-images.githubusercontent.com/28567623/49590889-43db5800-f96d-11e8-8d2a-2afed679c03b.png" /></p>

### Updating project files

Open `settings.json` file from `.vscode` and change value of `C_cpp.default.compilerPath` with path to `arm-none-eabi-gcc` location on your system:

<p align="center"><img width="800px" src="https://user-images.githubusercontent.com/28567623/49591150-f01d3e80-f96d-11e8-9d40-c496c968306a.png" /></p>

### Adding .mbedignore
https://os.mbed.com/docs/v5.10/tools/ignoring-files-from-mbed-build.html

In order to add support for CORE2 target and speed-up building of your project you need to exclude certain folders of `mbed-os` library from compilation. In your local `mbed-os` library root folder create new file `.mbedignore`. Open it and add following lines:

```
features/cellular/*
features/cryptocell/*
features/deprecated_warnings/*
features/device_key/*
features/lorawan/*
features/lwipstack/*
features/nanostack/*
features/netsocket/*
features/nfc/*
features/storage/*
features/unsupported/*
components/wifi/*
components/802.15.4_RF/*
components/storage/*
targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F407xG/device/TOOLCHAIN_GCC_ARM/STM32F407XG.ld
targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F407xG/device/TOOLCHAIN_GCC_ARM/startup_stm32f407xx.S
```

### Compilation tasks

To build and flash your firmware press `CTRL + SHIFT + P` and type `Tasks: Run Task` in Command Pallete. Select task of your choice. 

<p align="center"><img width="800px" src="https://user-images.githubusercontent.com/28567623/49592003-45f2e600-f970-11e8-89bb-25a4d7ff28b9.png" /></p>

You can add new tasks and customize existing ones by editing `task.json` file. 

<p align="center"><img width="800px" src="https://user-images.githubusercontent.com/28567623/49592056-6fac0d00-f970-11e8-8a71-51ae7c5e4a04.png" /></p>

### Debug

To debug:
* make sure you have stlink from texane installed on your system: https://github.com/texane/stlink/blob/master/README.md
* install extension: https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug
* compile and flash DEBUG firmware
* `CTRL + SHIFT + D` and click on `start debug` button

## mbed development

Documentation:
* [MBED OS Documentation](https://os.mbed.com/docs/v5.10/)
* [MBED OS API Doxygen](https://os.mbed.com/docs/v5.10/mbed-os-api-doxy/modules.html)
* [MBED OS GitHub](https://github.com/byq77/core2-mbed-template)
