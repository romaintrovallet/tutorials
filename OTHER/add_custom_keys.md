# Tutorial to perform DFU with custom keys

___

## 0) Requirements

You must have a working DFU in the first place.
If not, see NCS_UART_DFU or ZEPHYR_UART_DFU.

___

## 1) Key generation

There are multiple ways to generate keys for the MCUboot ([here](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/config_and_build/bootloaders/bootloader_signature_keys.html))
But for the simplicity of the tutorial
We will keep things simple with only ecdsa-p256 key type.

First create a folder where you store your keys.
Ex: `C:\Users\<username>\Documents\Stage\zephyr\keys\v1`
Named v1 because there might be other keys/type of keys

<details>
<summary><b>1) Private key in .pem</b></summary>

Has been tested and works well

```bash
nrfutil keys generate priv.pem
```

[Source](https://devzone.nordicsemi.com/guides/short-range-guides/b/software-development-kit/posts/nrf5-sdk-v17-1-0-secure-dfu-hands-on-tutorial)

</details>

<details>
<summary><b>2) Private key in .key</b></summary>

Has not been tested yet

</details>

___

## 2) Modify the app

***This step is best done when also Modifying the app on the main tutorial***

In your application folder, create a folder named `child_image`
**if already created, do not create another**

Into this folder create a file named `mcuboot.conf`
**if already created, do not create another**

Then you have multiple choices

<details>
<summary><b>1) Keep the keys in their folder</b></summary>

In that case, add these lines:

``` conf
# Path to custom key
CONFIG_BOOT_SIGNATURE_KEY_FILE="<absolute>/<path>/<to>/<key>/priv.pem"
# Fix to: undefined reference to 'rsa_pub_key_len'
CONFIG_BOOT_SIGNATURE_TYPE_ECDSA_P256=y
```

In my case:

- the path to their folder is : `C:/Users/<username>/Documents/Stage/zephyr/keys/v1/priv.pem`
- the path to the app folder is: `C:/ncs/myapps/ble_dfu_peripheral_lbs/priv.pem`

</details>

<details>
<summary><b>2) Place the keys in the mcuboot folder</b></summary>

In that case, place the private key in the mcuboot folder `C:/ncs/v2.5.2/bootloader/mcuboot/`

Then add these lines:

``` conf
# Path to custom key
CONFIG_BOOT_SIGNATURE_KEY_FILE="priv.pem"
# Fix to: undefined reference to 'rsa_pub_key_len'
CONFIG_BOOT_SIGNATURE_TYPE_ECDSA_P256=y
```

The path is shorter

</details>

___

## 3) Annexes

<details>
<summary><b>Serial Log after flash</b></summary>

```bash
*** Booting nRF Connect SDK v2.5.2 ***
I: Starting bootloader
I: Primary image: magic=unset, swap_type=0x1, copy_done=0x3, image_ok=0x3
I: Secondary image: magic=unset, swap_type=0x1, copy_done=0x3, image_ok=0x3
I: Boot source: none
I: Image index: 0, Swap type: none
I: Bootloader chainload address offset: 0xc000
*** Booting nRF Connect SDK v2.5.2 ***
Starting Bluetooth Peripheral LBS example
build time: Feb 15 2024 16:14:11
I: 2 Sectors of 4096 bytes
I: alloc wra: 0, fd0
I: data wra: 0, 1c
I: HW Platform: Nordic Semiconductor (0x0002)
I: HW Variant: nRF53x (0x0003)
I: Firmware: Standard Bluetooth controller (0x00) Version 141.732 Build 3324398027
I: No ID address. App must call settings_load()
Bluetooth initialized
I: Identity: CF:1E:65:8D:10:AC (random)
I: HCI: version 5.4 (0x0d) revision 0x2168, manufacturer 0x0059
I: LMP: version 5.4 (0x0d) subver 0x2168
Advertising successfully started
Connected
```

</details>

<details>
<summary><b>Serial Log in case of working DFU</b></summary>

```bash
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: test
*** Booting nRF Connect SDK v2.5.2 ***
I: Starting bootloader
I: Primary image: magic=unset, swap_type=0x1, copy_done=0x3, image_ok=0x3
I: Secondary image: magic=good, swap_type=0x2, copy_done=0x3, image_ok=0x3
I: Boot source: none
I: Image index: 0, Swap type: test
I: Starting swap using move algorithm.
I: Bootloader chainload address offset: 0xc000
*** Booting nRF Connect SDK v2.5.2 ***
Starting Bluetooth Peripheral LBS example
build time: Feb 15 2024 16:18:06
I: 2 Sectors of 4096 bytes
I: alloc wra: 0, fe8
I: data wra: 0, 0
I: HW Platform: Nordic Semiconductor (0x0002)
I: HW Variant: nRF53x (0x0003)
I: Firmware: Standard Bluetooth controller (0x00) Version 141.732 Build 3324398027
I: No ID address. App must call settings_load()
Bluetooth initialized
I: Identity: CF:1E:65:8D:10:AC (random)
I: HCI: version 5.4 (0x0d) revision 0x2168, manufacturer 0x0059
I: LMP: version 5.4 (0x0d) subver 0x2168
Advertising successfully started
Connected
```

</details>

<details>
<summary><b>Serial Log in case of failed DFU</b></summary>

```bash
I: Image index: 0, Swap type: revert
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: none
I: Image index: 0, Swap type: test
*** Booting nRF Connect SDK v2.5.2 ***
I: Starting bootloader
I: Primary image: magic=good, swap_type=0x2, copy_done=0x1, image_ok=0x1
I: Secondary image: magic=good, swap_type=0x2, copy_done=0x3, image_ok=0x3
I: Boot source: none
I: Image index: 0, Swap type: test
E: Image in the secondary slot is not valid!
I: Bootloader chainload address offset: 0xc000
*** Booting nRF Connect SDK v2.5.2 ***
Starting Bluetooth Peripheral LBS example
build time: Feb 15 2024 16:18:06
I: 2 Sectors of 4096 bytes
I: alloc wra: 0, fe8
I: data wra: 0, 0
I: HW Platform: Nordic Semiconductor (0x0002)
I: HW Variant: nRF53x (0x0003)
I: Firmware: Standard Bluetooth controller (0x00) Version 141.732 Build 3324398027
I: No ID address. App must call settings_load()
Bluetooth initialized
I: Identity: CF:1E:65:8D:10:AC (random)
I: HCI: version 5.4 (0x0d) revision 0x2168, manufacturer 0x0059
I: LMP: version 5.4 (0x0d) subver 0x2168
Advertising successfully started
Connected
```

</details>

___

## 4) Possible errors

### A) No `app_update.bin` in the `build/zephyr` folder

If the console doesn't provide any error but you can't find the `app_update.bin`.
Just delete the `build` folder in your application.
You will need to recreate a new build configuration (select the same options).
And the file should be here
