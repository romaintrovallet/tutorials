# Possible errors that could occur

## A) Error when flashing the Application

First verify that you have rightly plugged the Development Kit and that you have turned it on.

<details>
<summary><b>NCS</b></summary>

Then if a window is printed and asking to `Recover` the target
Press `Flash & Recover`
</details>
</br>

<details>
<summary><b>Vanilla Zephyr</b></summary>

Then add `--recover` to your command line, see example below

```bash
west flash -d apps/blinky/build/nrf5340dk_cpuapp/build_s --recover
```

</details>
</br>

## B) No `app_update.bin` or `zephyr.signed.bin` in the `build/zephyr` folder

If you work with **NCS**, the file is named `app_update.bin`.
If you work with **zephyrproject** (Vanilla Zephyr), the file is named `zephyr.signed.bin`.

If the console doesn't provide any error but you can't find the `app_update.bin` or `zephyr.signed.bin`.
Just delete the `build` folder in your application.
You will need to recreate a new build configuration (select the same options).
And the file should be here.
