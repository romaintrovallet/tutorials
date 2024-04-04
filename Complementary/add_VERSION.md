# Tutorial for DFU with Version of app

Version management is simple to set in place with Zephyr, here's how

First, it is recommended to perform at least 1 DFU.  
You can start with [ZEPHYR_UART_DFU](https://github.com/romaintrovallet/tutorials/blob/master/ZEPHYR_UART_DFU.md) if you are working with zephyrproject or
[NCS_UART_DFU](https://github.com/romaintrovallet/tutorials/blob/master/NCS_UART_DFU.md) if working with NCS.

I just present 1 way for application versionning, if alternatives are needed:
[Zephyr docs - Application version management](https://docs.zephyrproject.org/latest/build/version/index.html)

If you want to enable upgrade only / downgrade prevention, don't forget to check the [OPTIONNAL] steps.
___

## 1) Create a `VERSION` file

In your project root, create a file named `VERSION` (no extension).

Then add this inside of it:

```bash
VERSION_MAJOR = 1
VERSION_MINOR = 0
PATCHLEVEL = 1
VERSION_TWEAK = 0
EXTRAVERSION = stable
```

___

## 2) Build application

<details>
<summary><b>In NCS</b></summary>

Add the following lines to `src\main.c`

```c
#include <zephyr/kernel.h>
#include "app_version.h"
```

```c
printk("Hello version: %s \n",APP_VERSION_EXTENDED_STRING);
```

</details>
</br>
<details>
<summary><b>In Zephyr 'pure'</b></summary>

Not written yet

</details>
</br>

___

## 3) How to verify

<details>
<summary><b>With NCS (on computer)</b></summary>

Once the application is built, you can select the built folder in NCS

{$Picture of folder selected$}

Then in the Detailed window with the name of the built folder as title.
You can open `Output files` > `dfu_application.zip_manifest.json`
{$Picture of file selected$}

And now you can see the version of the application.
{$Picture of file opened$}

</details>
</br>
<details>
<summary><b>With MCUmgr (on target)</b></summary>

After the flash of the image.  
You can check the binary with :

```bash
mcumgr -c <name> image list
```

In the output you should have the version of the binary on target.

Then you can change the version of the file in your app.

Build the application (with the newly updated version).

Perform the DFU with MCUmgr (see [NCS_UART_DFU](https://github.com/romaintrovallet/tutorials/blob/master/NCS_UART_DFU.md#6-perform-dfu)
or [NCS_USB-CDC_DFU](https://github.com/romaintrovallet/tutorials/blob/master/NCS_USB-CDC_DFU.md#6-perform-dfu)
or [ZEPHRY_UART_DFU](https://github.com/romaintrovallet/tutorials/blob/master/ZEPHYR_UART_DFU.md#9-perform-dfu))

Then you can check again the images on target :

```bash
mcumgr -c <name> image list
```

And you should get 2 images with the 2 different versions.

</details>
</br>
<details>
<summary><b>At execution (on target)</b></summary>

In the output you should have the version of the binary on target.

Then you can change the version of the file in your app.

Build the application (with the newly updated version).

Perform the DFU with MCUmgr (see [NCS_UART_DFU](https://github.com/romaintrovallet/tutorials/blob/master/NCS_UART_DFU.md#6-perform-dfu)
or [NCS_USB-CDC_DFU](https://github.com/romaintrovallet/tutorials/blob/master/NCS_USB-CDC_DFU.md#6-perform-dfu)
or [ZEPHRY_UART_DFU](https://github.com/romaintrovallet/tutorials/blob/master/ZEPHYR_UART_DFU.md#9-perform-dfu))

Then you can check again the images on target :

```bash
mcumgr -c <name> image list
```

And you should get 2 images with the 2 different versions.

</details>
</br>

___

## 4) [OPTIONNAL] Enable Downgrade Prevention

### Details on the feature

Not written yet

### How to enable

`/!\ DISCLAIMER /!\`
This **ONLY** works with VERSION enabled.
It is recommended to verify the version of the app first
`/!\ DISCLAIMER /!\`

In `child_image/mcuboot.conf`, add this line:

```bash
CONFIG_MCUBOOT_DOWNGRADE_PREVENTION=y
```

___

## 5) [OPTIONNAL] Enable Upgrade Only

### Details on the feature

If the target is running App1 and updating to App2, enbling this feature will delete App1.
The second slot (slot1) will be empty and performing a revert directly on target will necessitate an upload.
Therefore, the "test" feature with MCUmgr is useless because it cannot revert at the second reset of the target.

### How to enable

`/!\ DISCLAIMER /!\`
This **ONLY** works with VERSION enabled.
It is recommended to verify the version of the app first
`/!\ DISCLAIMER /!\`

In `child_image/mcuboot.conf`, add this line:

```bash
CONFIG_BOOT_UPGRADE_ONLY=y
```

### Wich case to use

There is no relation between **Upgrade Only** and the Version of the app,
As it does not care about the Version of the app before suppressing the switched back image.
So there is no point of using it alone...

It is a great feature to **Downgrade Prevention**,
Because the Version of an Application is always increasing, here is an example:

If App1 (v1.1.0) is running on target and App2 (v1.2.0) is sent to target for an update

- Downgrade Prevention **ONLY**
  - App1 is *still* in the second slot of the Bootloader
  - When discovering that App2 has a problem (big enough), we *want to* switch back to App1 before fixing properly with App3
  - But as Downgrade Prevention is activated, trying to switch back will result in the suppression of the App1.
  - The proper "switch back to App1" is to create an App3(v1.2.1) that is App1 (revert of App2) (in simplified terms: v1.1.0 == v1.2.1)
  - Then fixing it properly with App4(v1.3.0)
- Downgrade Prevention **AND** Upgrade Only
  - App1 is *no longer* in the second slot of the Bootloader
  - When discovering that App2 has a problem (big enough), we *cannot* switch back to App1 before fixing properly with App3
  - The proper "switch back to App1" is to create an App3(v1.2.1) that is App1 (revert of App2) (in simplified terms: v1.1.0 == v1.2.1)
  - Then fixing it properly with App4(v1.3.0)

My point is, these 2 features are complementary.  
it's either you want the possibility to revert to the second image or not.

___

## 6) Known Issues

1) Building as none pristine doesn't work

    For starters, you should **NEVER** do a build that isn't pristine.

    In my testing, doing a 'classic' build only updates the version for the app calls (main.c).
    But for the version used by the bootloader is not (MCUmgr).

    So you can have examples with the app printing version X at execution (being the version you set in your `VERSION` file)
    and have the bootloader reading version Y (being the version at the previous pristine build)
