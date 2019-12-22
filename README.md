# platformio-cypress_psoc6
PlatformIO Configuration for Cypress PSoc6 based on Mbed Framework

## Hardware

PSoC6 BLE Prototyping Kit (CY8CPROTO-063-BLE)

JLink-EDU mini Debugger


## Setup
* Copy the repo content under ~/.platformio/platforms/cypress_psoc6
* Open VSCode
* Create a new PlatfomIO project folder by looking/selecting the CY8CPROTO-063-BLE board
* Modify the platformio.ini as below
* Create the launch.json file


## platformio.ini Config

```
[env:cy8cproto_063_ble]
platform = cypress_psoc6
board = cy8cproto_063_ble
framework = mbed

build_flags = -DPIO_FRAMEWORK_MBED_RTOS_PRESENT

debug_tool = jlink
upload_protocol = jlink

;debug_init_break = tbreak Reset_Handler

;build_flags = -Wl,-Map,output.map
```

## launch.json Debug Config

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

        {
            "name": "CM4 Debug J-Link",
            "cwd": "${workspaceRoot}",
            "executable": ".pio/build/cy8cproto_063_ble/firmware.elf",
            "request": "launch",
            "type": "cortex-debug",
            "servertype": "jlink",
            //"device": "CY8C6xx7_CM4_sect256KB",
            "device": "CY8C6xx7_CM4",
            "interface": "swd",
            "armToolchainPath": "~/.platformio/packages/toolchain-gccarmnoneeabi/bin",
            //"showDevDebugOutput":true,
            //"preLaunchTask": "Build All: Debug",
            "preLaunchCommands": [
                "set mem inaccessible-by-default off",
                "monitor speed 1000",
                "monitor clrbp",
                "monitor reset 0",
                "monitor halt",
                "monitor regs",
                "monitor speed 1000",
                "monitor flash breakpoints 1",
                "monitor semihosting enable"
            ],
            "postLaunchCommands": [
                "monitor clrbp",
                "monitor reset 2",
                "monitor halt",
                "monitor reset 0",
                "tbreak main",
                "monitor regs",
                "continue",             // uncomment for breaking at main, comment for breaking at first instruction
                "monitor halt"          // uncomment for breaking at main, comment for breaking at first instruction
            ],
            "postRestartCommands": [
                "monitor clrbp",
                "monitor reset 2",
                "monitor halt",
                "monitor reset 0",
                "tbreak main",
                "monitor regs",
                "continue",
                "monitor halt",
            ]
        },
        {
            "name": "CM0p Debug J-Link",
            "cwd": "${workspaceRoot}",
            "executable": ".pio/build/cy8cproto_063_ble/firmware.elf",
            "request": "launch",
            "type": "cortex-debug",
            "servertype": "jlink",
            //"device": "CY8C6xx7_CM4_sect256KB",
            "device": "CY8C6xx7_CM0p",
            "interface": "swd",
            "armToolchainPath": "~/.platformio/packages/toolchain-gccarmnoneeabi/bin",
            //"showDevDebugOutput":true,
            //"preLaunchTask": "Build All: Debug",
            "preLaunchCommands": [
                "set mem inaccessible-by-default off",
                "monitor speed 1000",
                "monitor clrbp",
                "monitor reset 0",
                "monitor halt",
                "monitor regs",
                "monitor speed 1000",
                "monitor flash breakpoints 1",
                "monitor semihosting enable"
            ],
            "postLaunchCommands": [
                "monitor clrbp",
                "monitor reset 2",
                "monitor halt",
                "monitor reset 0",
                "tbreak main",
                "monitor regs",
                "continue",             // uncomment for breaking at main, comment for breaking at first instruction
                "monitor halt"          // uncomment for breaking at main, comment for breaking at first instruction
            ],
            "postRestartCommands": [
                "monitor clrbp",
                "monitor reset 2",
                "monitor halt",
                "monitor reset 0",
                "tbreak main",
                "monitor regs",
                "continue",
                "monitor halt",
            ]
        }
    ]
}

```

## Blinky Test Program

```cpp
#include <mbed.h>

DigitalOut R(LED1);                  
DigitalOut B(LED2);                  


int main() {

  // put your setup code here, to run once:

R = 1;
B = 1;

  while(1) {
    // put your main code here, to run repeatedly:
    B = !B;
    wait(0.5);
    
  }
}
```

