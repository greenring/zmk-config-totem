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
- for the dongle build, flash `totem_dongle-seeeduino_xiao_ble-zmk.uf2` the same way.

## MONITORING BATTERY LEVELS ON WINDOWS (DONGLE SETUP)

The dongle sends keystrokes to your PC over USB, but it also fetches the left/right
battery levels over BLE and can expose them via a standard Bluetooth Battery Service
(this is what `CONFIG_ZMK_SPLIT_BLE_CENTRAL_BATTERY_LEVEL_PROXY`/`_FETCHING` in
`config/totem.conf` do). To read that from Windows, pair the dongle over Bluetooth
*in addition to* the USB connection — the USB link keeps handling keystrokes, the
Bluetooth link is only used to read battery levels.

1. Flash the updated firmware (this repo already enables the required config).
2. Put the dongle into BLE pairing mode: hold the `ADJ` layer and press `BT NEXT`
   (`&bt BT_NXT`) to advertise on an open Bluetooth profile.
3. On Windows: **Settings > Bluetooth & devices > Add device > Bluetooth**, and pair
   with "TOTEM Dongle". Keystrokes will keep coming through USB — you don't need to
   switch the active output.
4. Install [zmk-battery-center](https://github.com/kot149/zmk-battery-center), a
   system tray app that reads ZMK's split battery levels over BLE and shows
   central/left/right battery percentages with low-battery notifications:
   ```powershell
   powershell -ExecutionPolicy Bypass -Command "iex (irm 'https://raw.githubusercontent.com/kot149/zmk-battery-center/main/scripts/install_win.ps1')"
   ```
   or download the `*-setup.exe` installer from its
   [releases page](https://github.com/kot149/zmk-battery-center/releases).
5. Launch it — once the dongle is paired, it should appear in the tray with
   left/right/central battery percentages.

If Windows shows a stale battery percentage, re-open the Bluetooth device's "more
options" details page to force it to re-read, or unpair/re-pair the dongle.