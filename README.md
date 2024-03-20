# Read Me

You can find basic information about the Repository and how to use it.

Most of the informations (and even more) can be found in the Nordic Course Intermediate
[Nordic Course Intermediate - DFU/FOTA Section](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-8-bootloaders-and-dfu-fota/)

## Requirement

Before going any further you should either have:

- zephyrproject set up with zephyr SDK [Getting Started](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)
- nRF Connect SDK set up with NCS for VSCode [Install nRF Connect SDK](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/installation/install_ncs.html)

`/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\`
It's important to note that the 2 **should NOT be installed on the same computer** (outside of R&D purposes)
Choose with your target:

- **Nordic** Target => Install **nRF Connect SDK**
- Any **other brand** Target => Install **zephyrproject**

`/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\-/!\`
In addition of one of the above, you need:

- Serial Communication Port Reader (ex : TeraTerm / Putty / Termite)

Based on your "configuration" and the compatibility list below,
You can pick tutorials to realise on your configuration.

## Compatibility

All the main programs are compatible with:

- nRF5340dk
- Windows

### Main tutorial / Configuration

Before doing the main tutorial, verify the compatibility between:

- The main tutorials (rows)
- The SDK (cols)

| Compatibility Table | ZEPHYR_UART_DFU | NCS_UART_DFU | ... | NCS_USB_DFU | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **NCS** | Compatible | Compatible | ... | Compatible | ... | Compatible |
| **Zephyrproject** | Compatible | Not Compatible<sup>1</sup> | ... | N.A. | ... | Not Compatible<sup>1</sup>|

(<sup>1</sup>) => Zephyrproject isn't compatible with NCS made up samples

### Main tutorial / Complementary tutorial

Before adding complementary tutorials to the main ones, verify the compatibility between:

- The main tutorials (rows)
- The complementary tutorial (cols)

| Compatibility Table | ZEPHYR_UART_DFU | NCS_UART_DFU | ... | NCS_USB_DFU | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **Custom_Keys** | Compatible | Compatible | ... | Compatible | ... | Compatible |
| **Thingy91** | Not Compatible<sup>2</sup>| Compatible | ... | Not Compatible<sup>4</sup> | ... | Not Compatible<sup>3</sup>|

(<sup>2</sup>) => Thingy91 isn't compatible with 'Vanilla' Zephyr due to lack of its board

(<sup>3</sup>) => Thingy91 can't perform DFU with Bluetooth

(<sup>4</sup>) => DFU over USB (USB-CDC) needs a second usb port (only provided by Development Kits)

### Main tutorial / Fonctionnalities

You might want to add some functionnalities after.
So here is a table of tested functionnalities.

- The main tutorials (rows)
- The functionnalities (cols)

| Compatibility Table | ZEPHYR_UART_DFU | NCS_UART_DFU | ... | NCS_USB_DFU | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **TF-M** | Compatible | Compatible | ... | Not Compatible<sup>5</sup> | ... | Not Compatible<sup>5</sup> |
| **Revert** | Compatible | Compatible | ... | Compatible | ... | Compatible |
| **TF-M + Revert** | Compatible | Compatible | ... | Not Compatible<sup>5</sup> | ... | Not Compatible<sup>5</sup> |

(<sup>5</sup>) => Could not make it work
