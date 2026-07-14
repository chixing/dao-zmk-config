# ZMK Firmware for Dao keyboard

This fork's `dao42` branch is the source of truth for chixing's Dao42 firmware.
The original upstream also contains Dao44 support.

## Default keymap

### Dao42

Visual representation of the default keymap in keyboard-layout-editor: [KLE](http://www.keyboard-layout-editor.com/#/gists/67a81f6b83c65abcda5e7f32989a1688)

This layout is heavily inspired by [this](https://github.com/aroum/Watchman-layouts)

The current base layer uses plain `A`; holding `B` opens the F-key layer. There
are nine functional layers.

### Dao44

Visual representation of the default keymap in keyboard-layout-editor: [KLE](http://www.keyboard-layout-editor.com/#/gists/c6ba0634e5b92366be9f324775394e66)

This layout is heavily inspired by [this](https://github.com/KGOH/Jian-Info)

Because of current ZMK limitations, Dao44 keymap is in the branch [dao44](https://github.com/yumagulovrn/dao-zmk-config/tree/dao44)

## FAQ

- [FAQ](#faq)
  - [How to change the keymap?](#how-to-change-the-keymap)
  - [How to build locally?](#how-to-build-locally)
  - [How to flash the keyboard?](#how-to-flash-the-keyboard)
  - [How to pair halves?](#how-to-pair-halves)
  - [Problems](#problems)
    - [I'm getting File Transfer Error after copying firmware to the keyboard](#im-getting-file-transfer-error-after-copying-firmware-to-the-keyboard)

### How to change the keymap?

1. Edit [`config/dao.keymap`](config/dao.keymap), or use the [ZMK Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).
2. Commit the change.
3. Build locally as below, or push and download `firmware.zip` from GitHub Actions.

### How to build locally?

`config/west.yml` pins ZMK `v0.3`, the compatible release for the Ergonaut
DAO board module's legacy Zephyr hardware definition. In an initialized ZMK
toolchain/container workspace with this repo's `config` mounted at
`/workspace/config`:

```sh
west update
west zephyr-export
west build -p always -s zmk/app -d build/dao_left -b dao_left -- -DZMK_CONFIG=/workspace/config
west build -p always -s zmk/app -d build/dao_right -b dao_right -- -DZMK_CONFIG=/workspace/config
```

The outputs are `build/dao_left/zephyr/zmk.uf2` and
`build/dao_right/zephyr/zmk.uf2`.

### How to flash the keyboard?

1. Obtain `dao_left.uf2` and `dao_right.uf2`.
2. Flash one half at a time: switch it `OFF`, connect USB-C, then quickly press `RESET` twice.
3. When `NRF52BOOT` mounts, copy the matching UF2 to its root.
4. Wait for the drive to disconnect, then unplug that half.
5. Repeat for the other half. The left half owns the keymap, but keep both halves on the same build.

### How to pair halves?

1. Turn off the power for both halves (move slider to position `OFF`)
2. Turn on the power for both halves (move slider to position `ON`)
3. Press `RESET` button **once** on both halves **simultaneously**

### Problems

#### I'm getting File Transfer Error after copying firmware to the keyboard

It's OK. Proof: https://zmk.dev/docs/troubleshooting#file-transfer-error
