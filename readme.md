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

## BUILD MATRIX

The repo covers three setups. Flash the matching `.uf2` files from the Actions
artifact:

**1. Bluetooth pair (no dongle), with RGB**
- `totem_btleft rgbled_adapter` + `totem_btright rgbled_adapter` (both on XIAO).

**2. Dongle without display, with RGB**
- `totem_dongle rgbled_adapter` (XIAO dongle)
- halves: `totem_left rgbled_adapter` + `totem_right rgbled_adapter`.

**3. Dongle with display, no RGB**
- `totem_dongle_nn dongle_display` on a **nice!nano** (`nice_nano_v2`).
- halves: `totem_left` + `totem_right` (no `rgbled_adapter`).

The RGB status widget only exists where `rgbled_adapter` is included, so it is
absent from the display setup - the OLED shows status instead.

### Dongle display

The display uses [englmaxi/zmk-dongle-display](https://github.com/englmaxi/zmk-dongle-display)
(pinned to `v0.3` for this ZMK's LVGL 8). The dongle you bought is a **nice!nano**,
not a XIAO, so its firmware targets `nice_nano_v2` and the OLED sits on the
nice!nano's I2C (`pro_micro_i2c`) at address `0x3c`. If your dongle wires the
screen to different pins, adjust `totem_dongle_nn.overlay`.

Alongside the layer, modifiers, output status and bongo-cat widgets, it shows the
**battery level of the left and right halves** (via the split central battery
fetching enabled in `config/totem.conf`).