# Hello project

This project prints "Hello World" and a counter value via the UART output. It is configured for Arm Virtual Hardware, but other target hardware that provides a CMSIS Driver:USART can be easily added.

## Prerequisites

- Tools are installed using [vcpkg](https://learn.arm.com/learning-paths/microcontrollers/vcpkg-tool-installation/installation/). The latest version of the tools are available from [ARM tools Artifactory](https://www.keil.arm.com/artifacts/) are installed.
- Packs are listed in the file [`Hello.csolution.yml`](./Hello.csolution.yml) and installed using `cbuild`.

## Project Structure

The project is generated using the [CMSIS-Toolbox](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/README.md) and is defined in [`csolution`](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/docs/build-tools.md#csolution-invocation) format:

- [`Hello.csolution.yml`](./Hello.csolution.yml) lists the required packs and defines the hardware target and build-types (along with the compiler).
- [`Hello.cproject.yml`](./Hello.cproject.yml) defines the source files and the software components.
- [`hello.yml`](./.github/workflows/hello.yml) includes a matrix with targets, compilers, and build types which will be used for CI tests on GitHub.

## Generate a Project

To generate a project for a specific target-type, compiler, and build configuration execute the following commandline:

```txt
> cbuild Hello.csolution.yml --update-rte --packs --context Hello.Debug+CS300 --toolchain AC6 -r
```

- **--toolchain**   Specifies which compiler should be used for building the executable.
- **-r**            Forces a clean rebuild.
- **--packs**       Forces the download of required packs.
- **--update-rte**  Updates the run time environment.
- **Debug**         The selected build type.
- **CS300**         The selected target type.

## Execute Project

The project is configured for execution on [Arm Virtual Hardware](https://developer.arm.com/Tools%20and%20Software/Arm%20Virtual%20Hardware) which removes the requirement for a physical hardware board.  

- For running a specific project executable (generated for a specific context, build type, and compiler) use the following commandline:

### Example 1: For build type Debug, compiler AC6, and target type CM4

  ```txt
  > FVP_MPS2_Cortex-M4 -a ./out/Hello/CM4/Debug/AC6/Hello.axf -f ./FVP/FVP_MPS2_Cortex-M4.cfg --simlimit 60
```

### Example 2: For build type Release, compiler GCC, and target type CS310

  ```txt
  > FVP_Corstone_SSE-310 -a ./out/Hello/CS310/Release/GCC/Hello.axf -f ./FVP/FVP_Corstone_SSE-310.cfg --simlimit 60
  ```

- **-a**  Defines the path to the generated executable.
- **-f**  Specifies the configuration file for the used [`Arm Virtual Hardware`](https://arm-software.github.io/AVH/main/overview/html/index.html)
- **simlimit 60** Set the maximum time for the simulation

- [Keil Studio Cloud](https://studio.keil.arm.com/) integrates also the Arm Virtual Hardware VHT_Corstone_SSE-300_Ethos-U55 model. The steps to use the example are:
  - Start [Keil Studio Cloud](https://studio.keil.arm.com/) and login to the system using your account.
  - Use **File - Clone** and enter the URL: (https://github.com/Open-CMSIS-Pack/csolution-examples).
  - Select from the drop-down *Target hardware*: **Corstone SSE-300 (Cortex-M55, Ethos-U55)**
  - Click **Run project** which executes the project build step and then starts running on Arm Virtual Hardware.
