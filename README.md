# ZMK Config — splitkb Aurora Sofle v2 + pandaKB ZMK Wireless Receiver Dongle

Wireless split keyboard running ZMK with a **dongle** architecture:
- **Left half** — nice!nano v2
- **Right half** — nice!nano v2
- **Dongle** — nRF52840 & OLED
    - https://pandakb.com/shop/keyboard-kit/pandakb-zmk-split-keyboard-dongle/

This means both halves are *fully wireless* and your computer sees the dongle as a plain USB HID keyboard.

---

## Repository layout

```
aurorasofle_dongle/
├── build.yaml                          ← what to build
├── .github/workflows/build.yml         ← GitHub Actions CI
└── config/
    ├── west.yml                        ← ZMK + splitkb Aurora module pins (+ a few from pandaKB sofle_j_dongle implementation)
    ├── splitkb_aurora_sofle.conf       ← shared Kconfig options
    └── splitkb_aurora_sofle.keymap     ← keymap (edit this!)
└── zephyr/
    └── module.yml                      ← mandatory for non ZMK-standard shields (the dongle)
└── boards/shields/
    └── splitkb_aurora_sofle_dongle/
        ├── Kconfig.defconfig                    ← assignment of dongle as central role, split kb assignment, and OLED config
        ├── Kconfig.shield                       ← declaration of splitkb_aurora_sofle_dongle as a shield for ZMK
        ├── splitkb_aurora_sofle_dongle.conf     ← parameters for dongle to enable encoders etc.
        ├── splitkb_aurora_sofle_dongle.dtsi     ← import of josefadamcik sofle layout for physical mappings of keys, declaration of LEDs, encoders, OLED, etc.
        └── splitkb_aurora_sofle_dongle.overlay  ← import dtsi and define sleep for OLED and how to draw on screen
```
---

## Keymap overview

| Layer | How to reach | Purpose |
|-------|-------------|---------|
| **0 BASE** | Default | Standard typing |
| **1 LOWER** | Hold left thumb `LOWER` | Navigation, Shortcuts |
| **2 RAISE** | Hold right thumb `RAISE` | Function Keys, Symbols |
| **3 NUMPAD** | Hold left thumb `NUMPAD` | Numpad |

### Rotary encoders

| Half | Default (layer 0) | Lower layer | Raise layer |
|------|------------------|-------------|-------------|
| Left | Volume ↑↓ | Volume ↑↓ | Volume ↑↓ |
| Right | Next/Previous | CTRL+Left/CTRL+Right | Page Up/Down |

### ZMK Studio

Hold both encoder buttons simultaneously to unlock ZMK Studio.  
Then visit [studio.zmk.fm](https://studio.zmk.fm) with the dongle plugged in to remap keys live.

---

## Customising the keymap

Edit `config/splitkb_aurora_sofle.keymap`, commit, and push — GitHub Actions will rebuild automatically.

Key code reference: <https://zmk.dev/docs/codes>

---
