# Tutorial DFU over {$Techno$} with {$Sample$} sample

This tutorial will show:

- How to perform a DFU over {$Techno$}
- How to use {$Tool$}
- Using the {$Sample$} sample

Things omitted for the sake of simplicity:

{$Select required$}

- The TF-M mode (could not make it work with this example)
- The Secure mode (if working with Non-Secure mode(TF-M), Secure mod should be forgotten)
- Custom keys (another tutorial is available)
- Thingy91 as a target (nRF852840 on the Thingy91 shall not be used for this purpose)
- Thingy91 as a target (another tutorial is available)
- Other OS than Windows

Before starting this tutorial, it is recommended to read the following links:

{$Select required$}

- [Zephyr's doc on MCUboot](https://docs.mcuboot.com/readme-zephyr.html)
- [Nordic's doc on MCUmgr](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/zephyr/services/device_mgmt/mcumgr.html)

___

## 0) Requirements

This tutorial is made for NCS install.
It is not compatible with the zephyrproject install.

If you are interested by the zephyrproject / Vanilla Zephyr version
{$Select required$}
It can be found [here](link)  
It is not available yet.

With the global requirements, you should add the following:

{$Select required$}

- a phone with nRF Connect Application (available on Play Store / Apple Store)
- Go + MCUmgr ([Go Install](https://go.dev/doc/install) + [MCUmgr from Zephyr](https://docs.zephyrproject.org/latest/services/device_mgmt/mcumgr.html))

___

## 1) Create Application

In nRF Connect for VS Code, create a new application.
Select one of the 2 button

![Picture of the nRF Extension for VSCode where the place to click is higlighted](img/NCS/new_app.png)

You should have this window that pops up.  
We will create an app from an existing sample.  
Select the corresponding button

![Picture of VSCode where the place to click is higlighted](img/NCS/sample.png)

Then select the {$Sample$} sample by searching {$Sample_search$}

![Picture of VSCode where the place to click is higlighted](img/NCS/{$DFU$}/ble_lbs_sample.png)

Then save the app.
You should pick a high level folder because of the limit of 250 characters by CMake  
Furthermore, when you build the application you will have a `build` folder and within
a lots of folder and folder thus making the full path of certain files very long.

I choose this path for the example : `c:\ncs\apps\dfu_tutorial`
And I gave it the name {$app_naming$}

![Picture of VSCode with the path and the name given to the application](img/NCS/{$DFU$}/appli_saving.png)

![Picture of VSCode with the application ready to build](img/NCS/{$DFU$}/appli_saved.png)

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

![Picture of the main.c file modified](img/NCS/{$DFU$}/main.png)

Don't forget to save `src/main.c`!!

### B) prj.conf

Now open `prj.conf`

```bash
{$Write config$}
```

You should have something like this:

![Picture of the prj.conf file modified](img/NCS/{$DFU$}/conf.png)

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

## 3) Build Application

Now we need to configure the build settings.
Select one of the 2 button

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/{$DFU$}/build-1.png)

Select those 2 options and rename the output build folder to something recognizable.

{$Select required$}

At the time of making the tuto, a danger sign appears when selecting the board.
It's because of the secure and non-secure way to build the application.
If you have it too, look it up later

![Picture of the Build configuration with the place to modify the config higlighted](img/NCS/{$DFU$}/build-2.png)

If the build fails, try rebuild first (sometimes NCS needs a second build)
If it still fails, go to possible error section

This takes quite some time to generate.
But after the generation you should have something like that.

![Picture of the nRF Extension for VSCode with the visible build configuration](img/NCS/{$DFU$}/build-3.png)

___

## 4) Flash Application

Now is a good time to plug your device.

Once it is plugged and turned ON, you have 2 choices:

<details>
<summary><b>Open VSCode Serial Communication Port Reader</b></summary>

To see the log of our application, follow the steps:

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/{$DFU$}/output_conf-1.png)

For the next step the picture might not indicate what's to your screen.
Just go through the steps so you have the same configuration in the end.

![Picture of the serial configuration we have to select](img/NCS/output_conf_COM10-2.png)

![Picture of the terminal](img/NCS/{$DFU$}/output_log_pre-1.png)

</details>
</br>
<details>
<summary><b>Open your Serial Communication Port Reader</b></summary>

You have to find the used COM port (TeraTerm select it automatically)
And set the baud rate to `115200`

Once these 2 things are set, you are ready to flash

</details>
</br>

If ready, select the `Flash & Erase` command as presented below

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/{$DFU$}/flash.png)

{$Select required$}

If the flash was successful, you should see 3 things:

- A LED is blinking at a 1 sec rate
- The Serial log, itself composed of 2 parts:
  - The bootloader log
  - The application log
- Bluetooth
  - The device is visible
  - When connecting, LED2 is ON

The Serial log should be something like this

![Picture of the terminal where we can see the boot sequence](img/NCS/{$DFU$}/output_log_pre-2.png)

If you missed it, you can still press the `RESET` button
You should note the build time in the Serial Communication log
It's visible at the start of the application log

___

## 5) Build Application again

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

- the blinking LED (DK_LED1 -> DK_LED4) (line XX in `src/main.c`)
- the blinking rate (1000 -> 100) (line XX in `src/main.c`)
- the name of the target on Bluetooth Network (Nordic_LBS -> Quasar_Concept) (line XX in `prj.conf`)
- the LED (led0 -> led1) (line XX in `src/main.c`)
- the blinking rate (1000 -> 100) (line XX in `src/main.c`)

</details>
</br>

Rebuild by following the instructions below

![Picture of the nRF Extension for VSCode with the place to click higlighted](img/NCS/{$DFU$}/rebuild.png)

</details>
</br>
<details>
<summary><b>[OPTIONAL] New app</b></summary>

{$Additional details$}

</details>

___

## 6) Perform DFU

At this point, we use {$Tool$} to perform the DFU over {$Techno$}.
Just know that other tools exists
[List of Over The Air Update provided by Zephyr](https://github.com/zephyrproject-rtos/zephyr/blob/main/doc/services/device_mgmt/ota.rst)

{$Additional details$}

Go to your build folder (ex: `apps\dfu_tutorial\{$app_naming$}\{$build_naming$}`)
If you built **[OPTIONAL] New app** (in the **5) Build Application again**)
You must go to the new application build folder

Check for the presence of `zephyr\app_update.bin`

{$Write step by step$}

You should see the Bootloader swapping the image to another
And in the end the application loads with a more up to date Build Time

![Picture of the nRF Connect application](img/NCS/{$DFU$}/output_log_post.png)

You have now performed a DFU over {$Techno$} !!

{$Additional details$}

___

## 7) Possible errors

{$Select required$}

### A) Error when flashing the Application

First verify that you have rightly plugged the Development Kit and that you have turned it on.
Then if a window is printed and asking to `Recover` the target
Press `Flash & Recover`

### B) No `app_update.bin` in the `build/zephyr` folder

If the console doesn't provide any error but you can't find the `app_update.bin`.
Just delete the `build` folder in your application.
You will need to recreate a new build configuration (select the same options).
And the file should be here

### C) FAIL at Build => No configure step for 'tfm'

Relaunch the pristine build, it should work (no idea why it fails no the first try)

### D) Missing folders at Build

Just refresh the `Applications` bloc.

![Missing TF-M folder](img/errors/build_no_refresh.png)

If the refresh did not work, Rebuild as pristine.

### E) When `mcumgr` command => `Acces is denied`

In most cases, you forgot to close the Serial Communication Port

### F) When `mcumgr` command => `NMP timeout`

Try to execute simpler command like

```bash
mcumgr -c <name> echo hello
```

If it happened try the failed command a second time, it could work now.
If it doesn't work, this is generally because the MCUmgr config in the `prj.conf` is badly set.

### G) When `mcumgr image update` command => Stuck at 0%

Try deactivate mass storage on device:

- Open Jlink Commander  
![JLink Commander on Windows](img/errors/jlink.png)
- Execute this command:

```bash
MSDDisable
```

Then retry to perform update.
If still stuck, do the following steps in the right order:

- Flash & Erase the application
- A window could be printed while asking to `Recover` the target
- Press `Flash & Recover`
- Return to **Application transfer**