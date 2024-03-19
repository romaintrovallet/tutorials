# Compatibility

All the main programs are compatible with:

- nRF5340dk
- Windows

Before doing the main tutorial, verify the compatibility between:

- The main tutorials (rows)
- The SDK (cols)

| Compatibility Table | ZEPHYR_UART_DFU | ... | ... | ... | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **NCS** | Compatible | ... | ... | ... | ... | Compatible |
| **Zephyrproject** | Compatible | ... | ... | ... | ... | Not Compatible<sup>1</sup>|

(<sup>1</sup>) => Zephyrproject isn't compatible with NCS made up samples

Before adding complementary tutorials to the main ones, verify the compatibility between:

- The main tutorials (rows)
- The complementary tutorial (cols)

| Compatibility Table | ZEPHYR_UART_DFU | ... | ... | ... | ... | NCS_BLE_DFU |
| :------------------ | :-------------: | :-: | :-: | :-: | :-: | :---------: |
| **Custom_Keys** | Compatible | ... | ... | ... | ... | Compatible |
| **Thingy91** | Not Compatible<sup>2</sup>| ... | ... | ... | ... | Not Compatible<sup>3</sup>|

(<sup>2</sup>) => Thingy91 isn't compatible with 'Vanilla' Zephyr due to lack of its board

(<sup>3</sup>) => Thingy91 can't perform DFU with Bluetooth
