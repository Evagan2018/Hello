# AVH-Hello

This repository contains a CI project with a test matrix that uses [GitHub Actions](https://github.com/features/actions) on a [GitHub-hosted runner](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners) with an Ubuntu Linux system. The application module [`hello.c`](.\hello.c) prints "Hello World" with count value to the UART output. It is configured for [Arm Virtual Hardware - Fixed Virtual Platforms](https://arm-software.github.io/AVH/main/simulation/html/index.html) (AVH FVP), but it is easy to re-target it to hardware that provides a [CMSIS Driver:USART](https://arm-software.github.io/CMSIS_6/latest/Driver/group__usart__interface__gr.html).

The test matrix validates the application with GCC and Arm Compiler using a `Debug` and `Release` build. It builds and runs the application across the different [Arm Cortex-M processors](https://www.arm.com/products/silicon-ip-cpu?families=cortex-m) and various [Arm Corstone sub-systems](https://www.arm.com/products/silicon-ip-subsystems). It total it validates that 56 different variants execute correct on AVH FVP simulation models that represent a typical implementation of an Arm processor.

## Prerequisites

- The required tools are installed from [ARM Tools Artifactory](https://www.keil.arm.com/artifacts/) using [vcpkg](https://learn.arm.com/learning-paths/microcontrollers/vcpkg-tool-installation/installation/).
- The required Software Packs are installed using the [CMSIS-Toolbox](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/README.md) `cbuild` utility with the option `--packs`.

## Project Structure

The project is defined as [`CMSIS Solution Project`](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/docs/YML-Input-Format.md) that describes the build process for the [CMSIS-Toolbox](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/README.md).

Files and directories                          | Content
:----------------------------------------------|:----------
[`.github/workflows/`](./.github/workflows)    | GitHub Action file [`hello-ci.yml`](./.github/workflows/hello-ci.yml) defines a test matrix (with compiler, target, build types) that is iterated to build and run different variants of the application.
[`Board_IO/`](./Board_IO)                      | I/O re-targeting to a CMSIS-Driver UART interface.
[`FVP/`](./FVP)                                | Configuration files for the [AVH FVP](https://arm-software.github.io/AVH/main/simulation/html/index.html) simulation models.
[`RTE/Device/`](./RTE/Device/)                 | Includes for each device (target-type) the `RTE_Device.h` file with CMSIS-Driver. configuration.
[`RTE/CMSIS/`](./RTE/CMSIS)                    | RTOS configuration file `RTX_Config.h` used for all devices (targets).
[`Hello.csolution.yml`](./Hello.csolution.yml) | Lists the required packs and defines the hardware target and build-types.
[`Hello.cproject.yml`](./Hello.cproject.yml)   | Defines the source files and the software components.
[`cdefault.yml`](./cdefault.yml)               | Contains the setup for different compilers.
[`vcpkg-configuration.json`](./vcpkg-configuration.json) | Specifies the required tools for [vcpkg](https://learn.arm.com/learning-paths/microcontrollers/vcpkg-tool-installation/installation/); it is configured to install the latest versions.
[`main.c`](./main.c) / [`main.h`](./main.h)    | Application startup with CMSIS-RTOS
[`hello.c`](./hello.c)                         | Test Application

> **Note:**
>
> The privileged mode in `RTX_Config.h` is enabled, to allow the **USART** initialization.

The workflow allows to build and test the application on different host systems, for example local development computers that are Windows based and CI systems that run Linux.

## Build on Local Development Computer

To generate an application for a specific target-type, build-type, and compiler execute the following command line:

```txt
> cbuild Hello.csolution.yml --update-rte --packs --context Hello.Debug+CS300 --toolchain AC6 --rebuild
```

Parameters\Flags              | Description
:-----------------------------|:----------
**`--toolchain`**             | Specifies which compiler (GCC or AC6) is used to build the executable.
**`--rebuild`**               | Forces a clean rebuild.
**`--packs`**                 | Forces the download of required packs.
**`--update-rte`**            | Updates the Run-Time Environment (RTE directory).
**`Debug`**                   | Selects build-type (Debug or Release).
**`CS300`**                   | Selects target-type (CM0 is Cortex-M0, CS300 is Corstone-300).

## Execute on Local Development Computer

To execute an application on an [AVH FVP simulation model](https://arm-software.github.io/AVH/main/simulation/html/index.html) use the following command line:

```txt
> FVP_Corstone_SSE-300 -a ./out/Hello/CS300/Debug/AC6/Hello.elf -f ./FVP/FVP_Corstone_SSE-300.cfg --simlimit 60
```

Parameters\Flags              | Description
:-----------------------------|----------
**`-a`**                      |  Defines the path to the generated executable.
**`-f`**                      | Specifies the configuration file needed for the [Arm Virtual Hardware](https://arm-software.github.io/AVH/main/overview/html/index.html) model.
**`--simlimit`**              | Set the maximum time in seconds for the simulation.

> **Note:**
>
> - When using an FVP model for the first time you may need to configure firewalls for the Terminal output.

## Automated Build and Execute

The 