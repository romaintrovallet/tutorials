# Tutorial DFU over Bluetooth with Peripheral LBS sample

This tutorial will show:

- How to perform a DFU over Bluetooth
- How to use nRF Connect Application
- Using the BLE Peripheral LBS sample

Things omitted for the sake of simplicity:

- The TF-M mode (could not make it work with this example)
- Custom keys (another tutorial is available)
- Thingy91 as a target (nRF852840 on the Thingy91 shall not be used for this purpose)
- Other OS than Windows

Before starting this tutorial, it is recommended to read the following links:

- [Zephyr's doc on MCUboot](https://docs.mcuboot.com/readme-zephyr.html)

___

## 0) Requirements

This tutorial is made for NCS install.
It is not compatible with the zephyrproject install.

If you are interested by the zephyrproject / Vanilla Zephyr version
It is not available yet.

With the global requirements, you should add the following:

- a phone with nRF Connect Application (available on Play Store / Apple Store)

___

## 1) Create application

In nRF Connect for VS Code, create a new application.
Select one of the 2 button

![Picture of the nRF Extension for VSCode where the place to click is higlighted](img/NCS/1_new_app/new_app.png)

You should have this window that pops up.
We will create an app from an existing sample.
Select the correponding button

![Picture of VSCode where the place to click is higlighted](img/NCS/1_new_app/sample.png)

Then select the bt sample by searching `bluetooth/peripheral_lbs`

![Picture of VSCode where the place to click is higlighted](img/NCS/1_new_app/BT/bluetooth_sample.png)

Then save the app.
You should pick a high level folder because of the limit of 250 characters by CMake
Furthermore, when you build the application you will have a `build` folder and within
a lots of folder and folder thus making the full path of certain files very long.

I choose this path for the example : `c:\ncs\myapps\`

![Picture of VSCode where the place to click is higlighted](img/NCS/1_new_app/BT/appli_saving.png)

![Picture of path of the application in Windows Explorer](img/NCS/1_new_app/BT/appli_saving_v2.png)

![Picture of VSCode with the name given to the application](img/NCS/1_new_app/BT/appli_saving_v3.png)

___

## 2) Open Application

Now we need to open the app.
Select one of the 2 button

![Picture of the nRF Extension for VSCode where the place to click is higlighted](img/NCS/2_open_app/open_app_v1.png)

then select it in your windows browser

![Picture of the application folder to select in windows explorer](img/NCS/2_open_app/BT/open_app_v2.png)

___

## 2) Modify Application

### A) src/main.c

In your app folder, open `src/main.c`

Add this line of code in the main() => around line 190

```c
printk("build time: " __DATE__ " " __TIME__ "\n");
```

This will allow us to see the difference between old and new code after the update.
You should have something like this:

![Picture of the main.c file modified](img/NCS/3_modif_app/printk.png)

Don't forget to save !!

### B) prj.conf

Now open `prj.conf`

```bash
#Enable MCUBOOT bootloader build in the application
CONFIG_BOOTLOADER_MCUBOOT=y
#Include MCUMGR and the dependencies in the build
CONFIG_NCS_SAMPLE_MCUMGR_BT_OTA_DFU=y
```

You should have something like this:

![Picture of the prj.conf file modified](img/NCS/3_modif_app/proj_conf.png)

Don't forget to save !!

___

## 4) Build configuration

Now we need to configure the build settings.
Select one of the 2 button

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/4_build_app/BT/build_conf_v1.png)

Select those 2 options and rename the output build folder to something recognizable.

At the time of making the tuto, a danger sign appears when selecting the board.
It's because of the secure and non-secure way to build the application.
If you have it too, look it up later

![Picture of the Build configuration with the place to modify the config higlighted](img/NCS/4_build_app/BT/build_conf_v2.png)

This takes quite some time to generate.
But after the generation you should have something like that.

![Picture of the nRF Extension for VSCode with the visible build configuration](img/NCS/4_build_app/BT/build.png)

___

## 5) Flash the application

Now is a good time to plug your device.
If ready, flash your application as presented below.

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/5_flash_app/BT/flash.png)

___

## 6) Observe booting

To see the return of our application, follow the steps:

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/6_result/BT/output_1.png)

For the next step the picture might not indicate what's to your screen.
Just go through the steps so you have the same configuration in the end.

![Picture of the serial configuration we have to select](img/NCS/6_result/BT/output_2.png)  

![Picture of the terminal](img/NCS/6_result/BT/output_3.png)  

Now press the `Reset Button` on the devkit.
And observe the boot sequence on the terminal.

![Picture of the terminal where we can see the boot sequence](img/NCS/6_result/BT/output_4.png)

What's to note is the build time of the application

___

## 7) Perform DFU

### A) New build + Send file to phone

First you need to rebuild your application, by pressing the following button

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/7_DFU/BT/rebuild.png)

Now you should transfer the updated file to your phone.
It is located at `<path_to_app_foolder>\build\zephyr\app_update.bin`
I have chosen bluetooth.

![Picture of the file to transfer to your phone in windows explorer](img/NCS/7_DFU/BT/phone_transfer.png)

### B) Connect + Send file to device

<details>
<summary><b>Phone part</b></summary>

Now you have to open nRF Connect application on your phone.

Then connect to the the device

![Picture of the nRF Connect application with the list of available devices to connect](img/NCS/7_DFU/BT/phone/devices_available.jpg)

Then select `CONNECT` again in the top of the application.
You should now see the same things as the picture below.
Presss `DFU`

![Picture of the nRF Connect application with the place to click highlighted](img/NCS/7_DFU/BT/phone/device_connected.jpg)

You are now headed to your file system, choose the app_update file.
Then select `Test and Confirm` and `OK`

![Picture of the nRF Connect application](img/NCS/7_DFU/BT/phone/dfu_step_1.jpg)

You should see the graph like the picture below.

![Picture of the nRF Connect application](img/NCS/7_DFU/BT/phone/dfu_step_2.jpg)

</details>

Once it is done, we can head back to the terminal.

<details>
<summary><b>Serial Result</b></summary>

![Picture of the nRF Connect application](img/NCS/7_DFU/BT/boot_2.png)

Where we can see the build time is different than the previous one

</details>
