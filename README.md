# README

You can find basic information about the Repository and how to use it.

Most of the informations (and even more) can be found in the Nordic Course Intermediate  
[Nordic Course Intermediate - DFU/FOTA Section](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-8-bootloaders-and-dfu-fota/)

## Requirement

Before going any further you should either have:

- zephyrproject set up with zephyr SDK [Getting Started](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)
- nRF Connect SDK set up with NCS for VSCode [Install nRF Connect SDK](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/installation/install_ncs.html)

<details>
<summary><b>Choose between <u>zephyrproject</u> and <u>nRF Connect SDK</u></b></summary>

`/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\`  
It's important to note that the 2 **should NOT be installed on the same computer** (outside of R&D purposes)

Choose with your target:

- **Nordic** Target => Install **nRF Connect SDK**
- Any **other brand** Target => Install **zephyrproject**

`/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\`

</details>
</br>
In addition of one of the above, you need:

- Serial Communication Port Reader (ex : TeraTerm / Putty / Termite)

Based on your "configuration" and the compatibility list below,
You can pick tutorials to realise on your configuration.

## Compatibility

All the main programs are compatible with:

- nRF5340dk
- Windows

### Main tutorials / Configuration

Before doing the main tutorial, verify the compatibility between:

- The main tutorials (rows)
- The SDK (cols)

| Compatibility Table | ZEPHYR_UART_DFU | NCS_UART_DFU | ZEPHYR_USB-CDC_DFU | NCS_USB-CDC_DFU | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :----------: | :----------------: | :-------------: | :-: | :---------: |
| **NCS** | Compatible | Compatible | Compatible | Compatible | ... | Compatible |
| **zephyrproject** | Compatible | Compatible<sup>1</sup> | Compatible | N.A. | ... | Not Compatible<sup>2</sup>|

(<sup>1</sup>) => Uses a zephyr sample (not tested)

(<sup>2</sup>) => Zephyrproject isn't compatible with NCS made up samples

### Main tutorials / Complementary tutorial

Before adding complementary tutorials to the main ones, verify the compatibility between:

- The main tutorials (rows)
- The complementary tutorial (cols)

| Compatibility Table | ZEPHYR_UART_DFU | NCS_UART_DFU | ZEPHYR_USB-CDC_DFU | NCS_USB-CDC_DFU | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :----------: | :----------------: | :-------------: | :-: | :---------: |
| **Custom_Keys** | Compatible | Compatible | Compatible | Compatible | ... | Compatible |
| **Thingy91** | Not Compatible<sup>3</sup>| Compatible | Not Compatible<sup>4</sup> | Not Compatible<sup>4</sup> | ... | Not Compatible<sup>5</sup>|

(<sup>3</sup>) => Thingy91 isn't supported by 'Vanilla' Zephyr due to lack of its board

(<sup>4</sup>) => DFU over USB (USB-CDC) needs an SoC as USB peripheral [See compatible products](https://www.nordicsemi.com/-/media/Publications/WQ-Product-guide/Product-Guide_Nordic_2023.pdf)

(<sup>5</sup>) => Thingy91 can't perform DFU with Bluetooth

### Main tutorials / Fonctionnalities

You might want to add some functionnalities after.
So here is a table of tested functionnalities.

- The main tutorials (rows)
- The functionnalities (cols)

| Compatibility Table | ZEPHYR_UART_DFU          | NCS_UART_DFU | ZEPHYR_USB-CDC_DFU       | NCS_USB-CDC_DFU | ... | NCS_BLE_DFU                |
| :------------------ | :----------------------: | :----------: | :----------------------: | :-------------: | :-: | :------------------------: |
| **TF-M**          | Not Compatible<sup>6</sup> | Compatible | Not Compatible<sup>6</sup> | Compatible      | ... | Not Compatible<sup>6</sup> |
| **Revert**        | Compatible                 | Compatible | Compatible                 | Compatible      | ... | Compatible                 |
| **TF-M + Revert** | Not Compatible<sup>6</sup> | Compatible | Not Compatible<sup>6</sup> | Compatible      | ... | Not Compatible<sup>6</sup> |

(<sup>6</sup>) => Could not make it work

### SMP Server placement

All main tutorials use the SMP Server in the Application.
There are seperate tutorials that uses the SMP Server in the Bootloader.
It can be found in this folder.

## Credits

- [Nordic Course Intermediate - DFU/FOTA Section](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-8-bootloaders-and-dfu-fota/)
- [tree.nathanfriend](https://tree.nathanfriend.io/)
