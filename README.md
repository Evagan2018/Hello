# Hello project

This project prints "Hello World" and a counter value via the UART output. It is configured for Arm Virtual Hardware, but other target hardware that provides a CMSIS Driver:USART can be easily added.

## Prerequisites

### Tools

- [CMSIS-Toolbox 2.4.0](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/releases) or higher
- [Keil MDK 5.40](https://www2.keil.com/mdk5/)
  - Arm Compiler 6.18 (part of MDK, Eval Version sufficient for compilation)
  - Arm Virtual Hardware for Corstone-300 v11.24.24 (requires MDK - Professional)


### Packs

- Required packs are listed in the file [`Hello.csolution.yml`](./Hello.csolution.yml)

## Project Structure

The project is generated using the [CMSIS-Toolbox](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/README.md) and is defined in [`csolution`](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/docs/build-tools.md#csolution-invocation) format:


- [`Hello.csolution.yml`](./Hello.csolution.yml) lists the required packs and defines the hardware target and build-types (along with the compiler).
- [`Hello.cproject.yml`](./Hello.cproject.yml) defines the source files and the software components.

## Generate a Project

To generate a project for a specific target-type, compiler, and build configuration execute the following commandline:

```txt
> cbuild Hello.csolution.yml --update-rte --packs --context Hello.Debug+CS300 --toolchain AC6 -r
```

- **--toolchain**   Specifies which compiler should be used for building the executable.
- **-r**            Forces a clean rebuild
- **--packs**       Forces the download of required packs
- **--update-rte**  Updates the run time environment.



## Execute Project

The project is configured for execution on Arm Virtual Hardware which removes the requirement for a physical hardware board.  

- When using MDK-Professional, you may execute it with the command:

### For debug type
  ```txt
  > C:/Keil_v5/ARM/VHT/VHT_Corstone_SSE-300_Ethos-U55 -f vht-config.txt -a ./out/Debug/Hello.axf
```

### For release type
  ```txt
  > C:/Keil_v5/ARM/VHT/VHT_Corstone_SSE-300_Ethos-U55 -f vht-config.txt -a ./out/Release/Hello.axf
  ```

- **-a**  Defines the path to the generated executable 
- **-f**  Specifies the configuration file for the used [`Arm Virtual Hardware`](https://arm-software.github.io/AVH/main/overview/html/index.html)


- [Keil Studio Cloud](https://studio.keil.arm.com/) integrates also the Arm Virtual Hardware VHT_Corstone_SSE-300_Ethos-U55 model. The steps to use the example are:
  - Start [Keil Studio Cloud](https://studio.keil.arm.com/) and login to the system using your account.
  - Use **File - Clone** and enter the URL: (https://github.com/Open-CMSIS-Pack/csolution-examples).
  - Select from the drop-down *Target hardware*: **Corstone SSE-300 (Cortex-M55, Ethos-U55)**
  - Click **Run project** which executes the project build step and then starts running on Arm Virtual Hardware.
