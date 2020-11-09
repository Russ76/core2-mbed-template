# core2-mbed-template
Blinking leds project template for [Husarion CORE2](https://husarion.com/manuals/core2/), a board equipped with STM32 Cortex-M4 MCU. 
Project is prepared for development in Visual Studio Code with Mbed CLI tools.

> Based on version `mbed-os-5.14.2`

<p align="center"><img width="800px" src="https://cdn.shopify.com/s/files/1/2545/8446/products/CORE2-ROS_1024x1024@2x.png?v=1520001976" alt="CORE2-ROS"/></p>

## Prerequisites
You need to install following tools:
* [Visual Studio Code IDE](https://code.visualstudio.com/)
* [GNU Arm Embedded version 6 2017-q2 toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
* [stlink flasher](https://github.com/stlink-org/stlink/tree/master)

### Required Visual Studio Code extensions
* [Microsoft C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) (`ms-vscode.cpptools`)
* [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) (`marus25.cortex-debug`)

### Additional dependencies
We will also need following dependencies:
* Python 2.7.x
* Git
* Mercurial

Check [this](https://os.mbed.com/docs/mbed-os/v5.14/tools/manual-installation.html) link to find instruction on how to install this dependencies on your system.  

## Setting up a workspace directory and virtual environment

Install virtualenv package using pip2:
```bash
$ pip install virtualenv
```

Create a new folder `core2-mbed-workspace`. It will serve as workspace for your mbed projects.  Run:
```bash
$ mkdir core2-mbed-workspace && cd core2-mbed-workspace
```

In `core2-mbed-workspace` directory create python virtual environment (use full path to python version):

```bash
$ python2.7 -m virtualenv -p /usr/bin/python2.7 mbed_venv
```

## mbed-cli installation (python virtual environment)

Activate virtual environmet:

```bash
$ source mbed_venv/bin/activate
(mbed_venv) 
```

Install mbed-cli:
```bash
(mbed_venv)$ pip install mbed-cli
```

After installation set the path to the binary directory of your **GCC Arm Embedded Compiler** installation:

```bash
(mbed_venv)$ mbed config -G GCC_ARM_PATH "<your_path>\bin" 
```

Example for Windows:
```
(mbed_venv)$ mbed config -G GCC_ARM_PATH "C:\Program Files (x86)\GNU Tools ARM Embedded\6 2017-q2-update\bin" 
```

Example for Linux:
```
(mbed_venv)$ mbed config -G GCC_ARM_PATH ~\opt\gcc-arm-none-eabi-6-2017-q2-update\bin
```

To check current configuration run:

```bash
(mbed_venv)$ mbed config --list

```
## mbed-os library installation

1. Import mbed-os to `core2-mbed-workspace` directory:

```bash
(mbed_venv)$ mbed import mbed-os
```

2. The firmware uses Mbed OS version 5.14.2. In mbed-os directory run:

```bash
(mbed_venv)$ mbed update mbed-os-5.14.2
```

3. Install required python packages

```bash
(mbed_venv)$ cd mbed-os && pip install -r requirements.txt
```

4. Deactivate virtual environment
```bash
(mbed_venv)$ deactivate
```

## How to use firmware

Open Visual Studio Code, press `CTRL + SHIFT + P` and type `Git: Clone` in Command Pallet. Copy and paste the URL to this repo that you will find at GitHub page. 

You will be prompted to select your repo location. Choose `core2-mbed-workspace` directory.

### Create symbolic link to mbed-os directory

Inside `core2-mbed-template` run:

```bash
ln -s ../mbed-os ./mbed-os
```

### Update project files
Open `core2-mbed-template` in VSC. In `.vscode` directory find `settings.json` file and change the value of `C_cpp.default.compilerPath` with path to `arm-none-eabi-g++` location on your system:

Example (Windows):
```json
{
    "C_Cpp.default.compilerPath": "C:/Program Files (x86)/GNU Tools ARM Embedded/6 2017-q2-update/bin/arm-none-eabi-g++"
}
```

### Compilation tasks

To build and flash your firmware press `CTRL + SHIFT + P` and type `Tasks: Run Task` in Command Pallete. Here is the list of available tasks: 
* `BUILD (RELEASE)`
* `BUILD (DEBUG)`
* `FLASH FIRMWARE WHEN BOOTLOADER (RELEASE)`*
* `FLASH FIRMWARE WHEN BOOTLOADER (DEBUG)`  *
* `FLASH FIRMWARE NO BOOTLOADER (RELEASE)`  *
* `FLASH FIRMWARE NO BOOTLOADER (DEBUG)`    *
* `CLEAN DEBUG`
* `CLEAN RELEASE`

`*` *require ST-LINK programmer*

### Updating project files

Open `settings.json` file from `.vscode` and change value of `C_cpp.default.compilerPath` with path to `arm-none-eabi-g++` location on your system:

```json
{
    "C_Cpp.default.compilerPath": "C:/Program Files (x86)/GNU Tools ARM Embedded/6 2017-q2-update/bin/arm-none-eabi-g++"
}
```

You can add new tasks and customize existing ones by editing `task.json` file. 

#### Building and uploading firmware (BOOTLOADER)

The software bootloader allows the use of Husarion Cloud. You can find it in `TARGET_CORE2/bootloader_1_0_0_cortex.hex`. The instruction how to flash it can be found [here](https://husarion.com/manuals/core2/#updating-core2-bootloader).

To build firmware use `BUILD (RELEASE)` or `BUILD (DEBUG)` tasks.

To flash firmware connect ST-LINK to debug connector of CORE2 and use `FLASH FIRMWARE WHEN BOOTLOADER (RELEASE)` or `FLASH FIRMWARE WHEN BOOTLOADER (DEBUG)` task.

#### Building and uploading firmware (NO BOOTLOADER)
> Before proceeding with the following steps make sure you conducted mass erase of the memory and made all flash memory sectors are write unprotected.

If you do not want use the bootloader just remove this lines from mbed_app.json:
```json
    "target.mbed_app_start":"0x08010000",
    "target.mbed_rom_start":"0x08000000",
    "target.mbed_rom_size":"0x100000"
```

To build firmware use `BUILD (RELEASE)` or `BUILD (DEBUG)` tasks.

To flash firmware connect ST-LINK to debug connector of CORE2 and use `FLASH FIRMWARE NO BOOTLOADER (RELEASE)` or `FLASH FIRMWARE NO BOOTLOADER (DEBUG)` task.

#### Flashing firmware using `core2-flasher`

```bash
$ arm-none-eabi-objcopy -O ihex firmware.elf firmware.hex 
$ ./core2-flasher firmware.hex
```

You will find `firmware.elf` in `./BUILD/RELEASE` or `./BUILD/DEBUG`.

Here you can learn where to find `core2-flasher` for your system:
https://husarion.com/manuals/core2/#updating-core2-bootloader

#### Flashing firmware using `stm32loader`
https://github.com/byq77/stm32loader

This tool allows you to upload firmware using RPi connector.

If you have the bootloader the first two sectors are write protected. Before uploading new firmware you must unlock them (this will erase the bootloader):

```bash
$ sudo stm32loader -c <your_sbc> -u -W
```

To upload new firmware run:
```bash
$ sudo stm32loader -c <your_sbc> -e -v -w firmware.bin
```

### Debug

To debug:
* make sure you have stlink from texane installed on your system: https://github.com/texane/stlink/blob/master/README.md
* install extension: https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug
* compile and flash DEBUG firmware
* `CTRL + SHIFT + D` and click on `start debug` button

## mbed development

Documentation:
* [MBED OS Documentation](https://os.mbed.com/docs/v5.12/)
* [MBED OS API Doxygen](https://os.mbed.com/docs/v5.12/mbed-os-api-doxy/modules.html)
