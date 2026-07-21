<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/docs/images/TOTEM_logo_dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="/docs/images/TOTEM_logo_bright.svg">
  <img alt="TOTEM logo font" src="/docs/images/TOTEM_logo_bright.svg">
</picture>

# ZMK CONFIG FOR THE TOTEM SPLIT KEYBOARD

[Here](https://github.com/GEIGEIGEIST/totem) you can find the hardware files and build guide.\
[Here](https://github.com/GEIGEIGEIST/qmk-config-totem) you can find the QMK config for the TOTEM.

TOTEM is a 38 key column-staggered split keyboard running [ZMK](https://zmk.dev/) or [QMK](https://docs.qmk.fm/). It's meant to be used with a SEEED XIAO BLE or RP2040.


![TOTEM layout](/docs/images/TOTEM_layout.svg)



## HOW TO USE

- fork this repo
- `git clone` your repo, to create a local copy on your PC (you can use the [command line](https://www.atlassian.com/git/tutorials) or [github desktop](https://desktop.github.com/))
- adjust the totem.keymap file (find all the keycodes on [the zmk docs pages](https://zmk.dev/docs/codes/))
- `git push` your repo to your fork
- on the GitHub page of your fork navigate to "Actions"
- scroll down and unzip the `firmware.zip` archive that contains the latest firmware
- connect the left half of the TOTEM to your PC, press reset twice
- the keyboard should now appear as a mass storage device
- drag'n'drop the `totem_left-seeeduino_xiao_ble-zmk.uf2` file from the archive onto the storage device
- repeat this process with the right half and the `totem_right-seeeduino_xiao_ble-zmk.uf2` file.

## DONGLE DISPLAY

This config builds the dongle in two variants, so the same repo covers a plain
dongle and one with an OLED:

- `totem_dongle rgbled_adapter` &rarr; displayless dongle (unchanged).
- `totem_dongle totem_oled dongle_display rgbled_adapter` &rarr; dongle with a
  128x64 I2C OLED, using [englmaxi/zmk-dongle-display](https://github.com/englmaxi/zmk-dongle-display).
  `totem_oled` is a small local shield that just adds the OLED node, so the plain
  dongle build stays untouched.

Flash `totem_dongle totem_oled dongle_display-seeeduino_xiao_ble-zmk.uf2` onto the
dongle that has the screen; keep the plain `totem_dongle-...uf2` for a displayless one.

The screen is a standard 128x64 SSD1306 OLED wired to the XIAO's I2C pins (SDA/SCL
= D4/D5, plus 3V3 and GND) at address `0x3c`. Alongside the layer, modifiers,
output status and bongo-cat widgets, it shows the **battery level of the left and
right halves** (driven by the split central battery fetching already enabled in
`config/totem.conf`).