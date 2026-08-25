# dongle_display (vendored)

Vendored copy of the `dongle_display` shield from
[englmaxi/zmk-dongle-display](https://github.com/englmaxi/zmk-dongle-display)
at `v0.3` (the LVGL 8 / Zephyr 3.5 version matching this config's pinned ZMK),
MIT licensed. It replaces the west module of the same name so the battery
widget could be fixed locally.

Local changes vs upstream v0.3:

- `widgets/battery_status.c`: the widget no longer trusts the payload of the
  last battery event. The widget-listener macro keeps only a single pending
  state, so back-to-back events (a half reconnecting while the other notifies,
  a disconnect racing the initial read) overwrote each other and entries went
  stale or got applied while a different half's update was dropped. Every
  event now just triggers a re-read of all peripherals from the central's own
  state (`zmk_split_get_peripheral_battery_level()`), so each entry always
  shows that half's last known level.
- A disconnected half is shown as `--` instead of its entry being hidden, so
  it is always obvious which reading belongs to which half.
- The dongle's own entry only exists with
  `CONFIG_ZMK_DONGLE_DISPLAY_DONGLE_BATTERY=y`. Upstream also seeded entry 0
  from the central's battery/USB state at boot even with that option off,
  which put a phantom `0%`/`100%` USB-framed reading on a slot belonging to
  one of the halves.
- Build gating uses `CONFIG_ZMK_SPLIT_BLE_CENTRAL_BATTERY_LEVEL_FETCHING`
  instead of the removed `CONFIG_ZMK_BATTERY` symbol (upstream's gate needed a
  Kconfig shim in this repo; that shim is gone).
