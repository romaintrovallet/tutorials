# Possible errors that could occur when building with Vanilla Zephyr specifically

## A) Wrong path when `echo %ZEPHYR_BASE%`

First verify you are not in a virtual environment.
Then adapt and enter this command:

```bash
set ZEPHYR_BASE=<absolute>\<path>\<to>\zephyrproject\zephyr
```
