+++
title = 'ARM Cortex-M Debugging in VSCode'
date = 2024-08-06T20:53:28+01:00
description = "Setup and usage guide for Cortex-Debug VSCode extension"
+++
## Setup
1. Install the [Cortex-Debug](https://github.com/Marus/cortex-debug) extension in VSCode.
2. Install the [extra/arm-none-eabi-gdb](https://archlinux.org/packages/extra/x86_64/arm-none-eabi-gdb/) Arch package.
3. Create `launch.json` in project folder from this template for STM32F303CBT6 with a Nucleo debugger:
{{< highlight json >}}
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug (ST-Link)",
            "type": "cortex-debug",
            "request": "launch",
            "servertype": "openocd",
            "cwd": "${workspaceRoot}",
            "executable": "./.build/your_build_name.elf",
            "interface": "swd",
            "configFiles": [
                "/usr/share/openocd/scripts/interface/stlink.cfg",
                "/usr/share/openocd/scripts/target/stm32f3x.cfg"
            ],
            "device": "STM32F303CBT6", // This should match your MCU model
            "gdbPath": "/usr/bin/arm-none-eabi-gdb",
            "svdFile": "${workspaceFolder}/.vscode/STM32F303.svd" // Optional: Path to SVD file for peripheral register insight
        }
    ]
}
{{< /highlight >}}
4. Connect debugger to computer and press F5. Check the gdb-server output in terminal.
```
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
Info : Listening on port 50001 for tcl connections
Info : Listening on port 50002 for telnet connections
Info : clock speed 1000 kHz
Info : STLINK V2J42M27 (API v2) VID:PID 0483:374B
Info : Target voltage: 0.014155
Error: target voltage may be too low for reliable debugging
Error: init mode failed (unable to connect to the target)
```

## Usage
