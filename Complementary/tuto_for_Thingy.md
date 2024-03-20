# Tutorial for DFU with Thingy91

___

## 1) Build app

Create the build configuration (precise '_ns')

Launch the build

___

## 2) Flash app

Now is a good time to plug your device

With the Thingy91, you need to press the Main Button while turning it ON.
This triggers the Serial Recovery mode and make the Thingy programmable.

At this point you should open a Serial Communication Port Reader to see the incoming output.
Note that, at this point, you shouldn't see anything related to this application.

<details>
<summary><b>Use nRF Programmer</b></summary>

The nRF Programmer is available in the nRF Connect for Desktop app.

Install it and open it.

Select your device.

Select the file to program.

Press `Write.`

</details>
</br>
<details>
<summary><b>Use nRF Util</b></summary>

You can download this tool [here](https://www.nordicsemi.com/Products/Development-tools/nRF-Util)
Install the tool
Open a CMD and install `device`

```bash
nrfutil install device
```

Once it is done you can open a CMD in the build folder (ex:`apps\blinky\build\thingy91_9160_ns`)
Then enter this command:

```bash
nrfutil device program --firmware app_signed.hex
```

</details>
</br>
