# Tutorial DFU over {$Techno$} with {$Sample$} sample

This tutorial will show:

- How to perform a DFU over {$Techno$}
- How to use {$Tool$}
- Using the {$Sample$} sample

Things omitted for the sake of simplicity:

{$Select required$}

- Use of NCS for VSCode app
- Building the app as Non-Secure Processing Environment + TFM as Secure Processing Environment (could not make it work with Vanilla Zephyr)
- Building the app as Secure Processing Environment (if working as Non-Secure Processing Environment (TF-M), Secure Processing Environment should be forgotten)
- Custom keys (another tutorial is available)
- Thingy91 as a target (could not make it work with Vanilla Zephyr)
- Other OS than Windows

Before starting this tutorial, it is recommended to read the following links:

{$Select required$}

- [Zephyr's doc on MCUboot](https://docs.mcuboot.com/readme-zephyr.html)
- [Nordic's doc on MCUmgr](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/zephyr/services/device_mgmt/mcumgr.html)

___

## 0) Requirements

This tutorial is made for zephyrproject + zephyr SDK install
But it can be used with Zephyr version of NCS
Just replace `zephyrproject` with your toolchain version (ex:`v2.6.0`)
(Not recommended if you have a zephyrproject install)

If you are interested by the NCS version.  
{$Select required$}
It can be found [here](link)  
It is not available yet.

With the global requirements, you should add the following:

{$Select required$}

- a second USB cable
- Go + MCUmgr ([Go Install](https://go.dev/doc/install) + [MCUmgr from Zephyr](https://docs.zephyrproject.org/latest/services/device_mgmt/mcumgr.html))

___

## 1) Create Application

Go to your zephyrproject install.
Go to this path : `zephyrproject\zephyr\samples\basic`
Copy the `{$sample$}` folder

Paste it in your app folder (ex: `zephyrproject\dfu_tutorial\{$sample$}`)
Rename it to a more appropriate name (ex: `zephyrproject\dfu_tutorial\{$app_naming$}`)
For the next steps, we will assume you pick the example folder

This will be the application we are working with.

___

## 2) Modify Application

At this point you should have something like this:

```bash
.
└── dfu_tutorial/
    └── {$app_naming$}/
        ├── src/
        │   └── main.c
        ├── .gitignore
        ├── CMakeLists.txt
        ├── prj.conf
        ├── README.rst
        └── sample.yaml
```

To make the DFU work, we will need to modify the application

### A) src/main.c

In your app folder, open `src/main.c`

Add this line of code in the main() => around line XX

```c
printk("build time: " __DATE__ " " __TIME__ "\n");
```

This will allow us to see the difference between old and new code after the update.
You should have something like this:

![Picture of the main.c file modified](img/ZEPHYR/{$DFU$}/main.png)

Don't forget to save `src/main.c`!!

### B) prj.conf

Now open `prj.conf` and copy-paste the following lines.

```bash
{$Write config$}
```

You should have something like this:

![Picture of the prj.conf file modified](img/ZEPHYR/{$DFU$}/conf.png)

Don't forget to save `prj.conf`!!

{$Additional details$}

At this point you should have something like this:

```bash
.
└── dfu_tutorial/
    └── {$app_naming$}/
        ├── src/
        │   └── main.c (M)
        ├── .gitignore
        ├── CMakeLists.txt
        ├── prj.conf (M)
        ├── README.rst
        └── sample.yaml
```

___

## 3) Command Line config

In this tutorial we will use the command line
Open a terminal in the parent folder of `zephyrproject`

You will need to build applications and bootloaders until Step 7 included.
If, for whatever reason, you cannot complete the whole tutorial in one time.
You need to make this ***Step all over again.***

In the following, it will be called the **MAIN_TERMINAL**

Enter this command:

```bash
echo %ZEPHYR_BASE%
```

and it should return something like this:

```bash
<absolute>\<path>\<to>\zephyrproject\zephyr
```

If not go to error section

Still in the **MAIN_TERMINAL**, enter this command:

```bash
zephyrproject\.venv\Scripts\activate.bat
```

You are now in the zephyr virtual environment.
Keep your **MAIN_TERMINAL** open.

Enter the following commands:

```bash
cd zephyrproject
```

```bash
west update
```

```bash
west zephyr-export
```

Once done, keep it in your background and do not close it

___

## 4) Build Application

In the **MAIN_TERMINAL**

Enter this command :

```bash
west build -b nrf5340dk_nrf5340_cpuapp dfu_tutorial/{$app_naming$} -d dfu_tutorial/{$app_naming$}/{$build_naming$}
```

If the build fails, try rebuild first (sometimes Zephyr needs a second build)
If it still fails, go to possible error section

___

## 5) Build Bootloader

In the **MAIN_TERMINAL**

Enter this command :

```bash
west build -b nrf5340dk_nrf5340_cpuapp bootloader/mcuboot/boot/zephyr -d {$build_boot_naming$}
```

___

## 6) Flash Application

Now is a good time to plug your device.

Once it is plugged and turned ON, enter this command in the **MAIN_TERMINAL**:

```bash
west flash -d dfu_tutorial/{$app_naming$}/{$build_naming$}
```

If it doesn't flash, go to possible errors sections

At this point you should open a Serial Communication Port Reader to see the incoming output.

You have to find the used COM port (TeraTerm select it automatically).
And set the baud rate to `115200`.
Note that, at this point, you shouldn't see anything related to this application.

Once these 2 things are set, you are ready to flash the bootloader

___

## 7) Flash Bootloader

In the **MAIN_TERMINAL**

Enter this command :

```bash
west flash -d {$build_boot_naming$}
```

If the flash was successful, you should see 2 things:

- A LED is blinking at a 1 sec rate
- The Serial log, itself composed of 2 parts:
  - The bootloader log
  - The application log

If you missed it, you can still press the `RESET` button
You should note the build time in the Serial Communication log
It's visible at the start of the application log

![Tera Term log](img/ZEPHYR/1_result/image.png)

___

## 8) Build Application again

At this point, you have a working bootloader and application
Now we will update the application with a new version of the same application

But you can also use another application
Just make sure to have (at least) the same configuration as presented in step 1

For this part, we will just rebuild (it's enough to see the difference)
But if you want a more visual approach, there are possibilities available below

<details>
<summary><b>Rebuild the same app</b></summary>
</br>
<details>
<summary><b>[OPTIONAL] Modify the app</b></summary>

You can modify the app to bring a more visually updated approach
Here are some examples :

{$Select required$}

- the blinking LED (led0 -> led1) (line XX in `src/main.c`)
- the blinking rate (1000 -> 100) (line XX in `src/main.c`)
- the name of the USB device (add following lines in `prj.conf`)

```bash
# See effect of DFU
CONFIG_USB_DEVICE_PRODUCT="Zephyr DFU sample"
```

</details>
</br>

In the **MAIN_TERMINAL**

Enter this command :

```bash
west build -b nrf5340dk_nrf5340_cpuapp dfu_tutorial/{$app_naming$} -d dfu_tutorial/{$app_naming$}/{$build_naming$} -p
```

</details>
</br>
<details>
<summary><b>[OPTIONAL] New app</b></summary>

Follow the **A) Copy sample** in the **1) Create Application**
Instead get the `zephyrproject\zephyr\samples\hello_world`
Copy and rename it to `zephyrproject\dfu_tutorial\{$app_naming$}_hw`

Follow the same modification in the **2) Modify Application**
and add this library in the `zephyrproject\dfu_tutorial\{$app_naming$}_hw\src\main.c`

```c
#include <zephyr/kernel.h>
```

In the **MAIN_TERMINAL**

Then build it with this command

```bash
west build -b nrf5340dk_nrf5340_cpuapp dfu_tutorial/{$app_naming$}_hw -d dfu_tutorial/{$app_naming$}_hw/{$build_naming$}
```

</details>

___

## 9) Perform DFU

At this point, we use {$Tool$} to perform the DFU over {$Techno$}.
Just know that other tools exists
[List of Over The Air Update provided by Zephyr](https://github.com/zephyrproject-rtos/zephyr/blob/main/doc/services/device_mgmt/ota.rst)

{$Additional details$}

Go to your build folder (ex: `zephyrproject\dfu_tutorial\{$app_naming$}\{$build_naming$}`)  
If you built **[OPTIONAL] New app** (in the **8) Build Application again**)
You must go to the new application build folder

Check for the presence of `zephyr\zephyr.signed.bin`

{$Write step by step$}

You should see the Bootloader swapping the image to another
The application loads with a more up to date Build Time

![Shows the DFU log in TeraTerm](img/ZEPHYR/2_DFU/image-2.png)

You have now performed a DFU over {$Techno$} !!

{$Additional details$}
