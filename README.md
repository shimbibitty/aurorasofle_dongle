# ZMK Config — splitkb Aurora Sofle v2 + nice!nano v2 Dongle

Wireless split keyboard running ZMK with a **dongle** architecture:
- **Left half** — nice!nano v2, BLE peripheral
- **Right half** — nice!nano v2, BLE peripheral
- **Dongle** — nice!nano v2 plugged into your PC via USB, acts as central BLE host

This means both halves are *fully wireless* and your computer sees the dongle as a plain USB HID keyboard.

---

## Repository layout

```
zmk-config-aurora-sofle/
├── build.yaml                          ← what to build
├── .github/workflows/build.yml         ← GitHub Actions CI
└── config/
    ├── west.yml                        ← ZMK + splitkb Aurora module pins
    ├── splitkb_aurora_sofle.conf       ← shared Kconfig options
    └── splitkb_aurora_sofle.keymap     ← keymap (edit this!)
```

---

## First-time setup

### 1. Fork this repo on GitHub

Everything builds in the cloud — no local toolchain needed.

### 2. Enable GitHub Actions

Go to the **Actions** tab in your fork and click "I understand my workflows, go ahead and enable them."

### 3. Trigger a build

Push any change (even a whitespace edit) or click **Run workflow** in the Actions tab.  
Download the `firmware` artifact from the completed run.

---

## Flashing order (important!)

> ⚠️ Only plug in **one device at a time** when flashing. Power off the other halves.

### Step 1 — Settings reset (do this once, or whenever you have pairing issues)

Flash `settings_reset-nice_nano_v2-zmk.uf2` to **each** of the three nice!nanos in turn:

1. Double-tap the reset button → it mounts as a USB drive
2. Drag `settings_reset-…uf2` onto the drive
3. Wait for it to unmount, then move on to the next board

### Step 2 — Flash the peripherals (halves)

| File | Target |
|------|--------|
| `splitkb_aurora_sofle_left-nice_nano_v2-zmk.uf2` | Left half |
| `splitkb_aurora_sofle_right-nice_nano_v2-zmk.uf2` | Right half |

Flash each half the same way: double-tap reset → drag the file.

### Step 3 — Flash the dongle

Flash `splitkb_aurora_sofle_dongle-nice_nano_v2-zmk.uf2` to the dongle nice!nano.

### Step 4 — Pair

1. Plug the dongle into your PC.
2. Power on the left half.
3. Power on the right half.
4. Both halves should auto-connect to the dongle within a few seconds (green LED solid).

---

## Keymap overview

| Layer | How to reach | Purpose |
|-------|-------------|---------|
| **0 QWERTY** | Default | Standard typing |
| **1 LOWER** | Hold left thumb `LOWR` | Symbols, F-keys |
| **2 RAISE** | Hold right thumb `RAIS` | Navigation, numpad |
| **3 ADJUST** | Hold both `LOWR`+`RAIS` | Bluetooth profiles, output, power |

### Rotary encoders

| Half | Default (layer 0) | Lower layer | Raise layer |
|------|------------------|-------------|-------------|
| Left | Volume ↑↓ | Volume ↑↓ | Volume ↑↓ |
| Right | Page Up/Down | Left/Right arrow | Page Up/Down |

### Bluetooth / dongle controls (ADJUST layer)

| Key | Action |
|-----|--------|
| `BT_SEL 0–4` | Select BLE profile on dongle |
| `BT_CLR` | Clear current BLE profile |
| `OUT_USB` | Force USB output |
| `OUT_BLE` | Force BLE output |
| `EP_TOG` | Toggle external power (LEDs, etc.) |

### ZMK Studio

Hold both encoder buttons simultaneously to unlock ZMK Studio.  
Then visit [studio.zmk.fm](https://studio.zmk.fm) with the dongle plugged in to remap keys live.

---

## Customising the keymap

Edit `config/splitkb_aurora_sofle.keymap`, commit, and push — GitHub Actions will rebuild automatically.

Key code reference: <https://zmk.dev/docs/codes>

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Halves won't connect to dongle | Flash `settings_reset` to all three, then reflash firmware in order |
| Keys from one half not working | Check that half is powered on and the dongle is plugged in first |
| Keymap not updating | Make sure you pushed to the correct branch and Actions completed |
| `ZMK Studio` greyed out | Make sure dongle firmware was built with `CONFIG_ZMK_STUDIO=y` (it is, by default) |
