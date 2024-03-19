# Read Me

You can find basic information about the Repository and how to use it.

Most of the informations can be found in the Nordic Course available [here](https://academy.nordicsemi.com/courses/nrf-connect-sdk-intermediate/lessons/lesson-8-bootloaders-and-dfu-fota/)

## Requirement

Before going any further you should either have:

- zephyrproject set up with zephyr SDK
- nRF Connect SDK set up with NCS for VSCode

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

| Compatibility Table | ZEPHYR_UART_DFU | ... | ... | ... | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **NCS** | Compatible | ... | ... | ... | ... | Compatible |
| **Zephyrproject** | Compatible | ... | ... | ... | ... | Not Compatible<sup>1</sup>|

(<sup>1</sup>) => Zephyrproject isn't compatible with NCS made up samples

### Main tutorial / Complementary tutorial

Before adding complementary tutorials to the main ones, verify the compatibility between:

- The main tutorials (rows)
- The complementary tutorial (cols)

| Compatibility Table | ZEPHYR_UART_DFU | ... | ... | ... | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **Custom_Keys** | Compatible | ... | ... | ... | ... | Compatible |
| **Thingy91** | Not Compatible<sup>2</sup>| ... | ... | ... | ... | Not Compatible<sup>3</sup>|

(<sup>2</sup>) => Thingy91 isn't compatible with 'Vanilla' Zephyr due to lack of its board

(<sup>3</sup>) => Thingy91 can't perform DFU with Bluetooth

### Main tutorial / Fonctionnalities

You might want to add some functionnalities after.
So here is a table of tested functionnalities.

- The main tutorials (rows)
- The functionnalities (cols)

| Compatibility Table | ZEPHYR_UART_DFU | ... | ... | ... | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **TF-M** | Compatible | ... | ... | ... | ... | Not Compatible |
| **Revert** | Compatible | ... | ... | ... | ... | Not Compatible<sup>1</sup>|
