# Problems

## AVH-CS300

- compiler: GCC ends in FVP with Hardfault

**Reason:**

``` txt
                 initialise_monitor_handles (0x10005658):
0x100002E8 2016      MOVS     r0,#0x16
0x100002EA A131      ADR      r1,0x100003b0
0x100002EC BEAB      BKPT     #0xab                  <-- Breakpoint instruction
0x100002EE 4830      LDR      r0,0x100003b0
0x100002F0 6841      LDR      r1,[r0,#4]
```

## AVH-CM0plus

- compiler: GCC linker issues warning
ld.exe: warning: C:\w\csolution-examples\Hello\out\Hello\AVH-CM0plus\Debug\Hello.elf has a LOAD segment with RWX permissions

- Simulation does not stop with EOT (configuration setting missing)

## Inconsistent

This component is defined with vendor Arm (for Corstone targets), and vendor Keil (for Cortex targets)
    - component: Device:Startup&C Startup


## Build fails
 The following contexts cannot build:
   **CM33_MPS3** (`device: ARM::IOTKit_CM33_MPS3`) and **CM33_FP_MPS3** (`device: ARM::IOTKit_CM33_FP_MPS3`)



## FVP configuration settings
   
   The privileged mode in **.\Hello\RTE\CMSIS\RTX_Config.h** must be enabled, to allow the **USART** initialization when
   **FVP_Corstone_SSE-300**, **FVP_Corstone_SSE-310**, and **FVP_Corstone_SSE-315** are used.
  
   **#define OS_PRIVILEGE_MODE           1**

## FVP issues
   No **UART** output is shown for executables which are using the following **FVPs**:
   ``` txt
   FVP_MPS2_Cortex-M0
   FVP_MPS2_Cortex-CM0plus
   FVP_MPS2_Cortex-M3
   FVP_MPS2_Cortex-M4
   FVP_MPS2_Cortex-M7
   FVP_MPS2_Cortex-M23
   FVP_MPS2_Cortex-M33   
   ``` 