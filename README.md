<div align="center">

# 🔊 PRJ-FW-4000-2019-ESP32-BTAudioStreamTest

### ESP32 Classic-Bluetooth (A2DP Sink) Audio Streaming Test

**Designed by [Ibrar Ahmad](https://github.com/hiibrarahmad)**

[![MCU](https://img.shields.io/badge/MCU-ESP32-00c8ff?style=for-the-badge)](#)
[![Framework](https://img.shields.io/badge/Framework-ESP--IDF-22c55e?style=for-the-badge)](#)
[![Profile](https://img.shields.io/badge/BT%20Profile-A2DP%20Sink%20%2B%20AVRCP-ff6b35?style=for-the-badge)](#)

[![Last Commit](https://img.shields.io/github/last-commit/hiibrarahmad/PRJ-FW-4000-2019-ESP32-BTAudioStreamTest?style=for-the-badge&color=0891b2&label=Last%20Commit)](../../commits/main)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

</div>

---

## 📖 Overview

A test project written to verify the ESP-IDF Bluedroid **A2DP sink** stack against a real **I2S** audio path: the ESP32 advertises itself over classic Bluetooth as `ESP32-SPEAKER`, accepts an audio stream from any paired phone/PC, and writes the decoded PCM straight out over I2S to an external DAC/amplifier. **AVRCP** (controller role) rides alongside it for play/pause/track-skip feedback and track metadata (title/artist/album) logged over serial.

This is a **testing project**, not a finished product — written in 2019 to confirm the streaming + I2S output path worked end-to-end, not to build a polished Bluetooth speaker.

---

## 🎯 What It Does

1. Initializes NVS, the Bluetooth controller, and Bluedroid (classic BT only — BLE memory is released).
2. Advertises as `ESP32-SPEAKER`, connectable and discoverable, no PIN (Just Works pairing).
3. Registers an **A2DP sink** — incoming decoded audio bytes are written directly to `i2s_write()`.
4. Registers an **AVRCP controller** — logs connection state, requests title/artist/album metadata on connect, and re-registers for playback-status and volume-change notifications after each event (`ESP_AVRC_RN_PLAY_STATUS_CHANGE`, `ESP_AVRC_RN_VOLUME_CHANGE`).
5. Configures I2S peripheral 0 as master/TX, 44.1kHz, 16-bit, stereo.

## 🔌 I2S Wiring

| ESP32 GPIO | I2S Signal | DAC/Amp Pin |
|---|---|---|
| GPIO27 | BCLK (bit clock) | BCK / SCK |
| GPIO26 | LRCK (word select) | LRCK / WS |
| GPIO25 | DOUT (data out) | DIN / SD |

Tested against a simple external I2S DAC/amp module (e.g. MAX98357A or PCM5102A class). Pins are `#define`d at the top of `main/main.c` if you need to remap them.

---

## 🛠️ Build & Flash

```sh
# Set up the ESP-IDF environment (once per shell)
. $HOME/esp/esp-idf/export.sh

# Set target and build
idf.py set-target esp32
idf.py build

# Flash + monitor
idf.py -p /dev/ttyUSB0 flash monitor
```

Pair your phone/PC with `ESP32-SPEAKER`, play audio, and it streams straight through to whatever's on the I2S pins above.

---

## 📁 Repository Structure

```
PRJ-FW-4000-2019-ESP32-BTAudioStreamTest/
│
├── main/
│   ├── main.c              ← Entire application: BT init, A2DP sink, AVRCP, I2S
│   ├── CMakeLists.txt
│   └── Kconfig.projbuild
│
├── CMakeLists.txt          ← Top-level ESP-IDF project file
├── sdkconfig.defaults       ← Baseline project config
├── sdkconfig.ci.test        ← CI build config
├── .devcontainer/           ← ESP-IDF dev container (VS Code)
└── LICENSE
```

`main.c` is a single self-contained file — the whole A2DP sink + I2S + AVRCP flow lives there, no other source files are compiled (see `main/CMakeLists.txt`).

---

## 🔗 Links

| Resource | URL |
|----------|-----|
| 👤 Designer | [github.com/hiibrarahmad](https://github.com/hiibrarahmad) |

---

<div align="center">

**PRJ-FW-4000-2019-ESP32-BTAudioStreamTest**

*ESP32 A2DP Sink / I2S Audio Streaming Test · Written 2019 by Ibrar Ahmad*

© 2019 Ibrar Ahmad. MIT Licensed.

</div>
