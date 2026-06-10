# 🚁 Turbo-FPV

> A compact 5-inch FPV racing drone built for ~$190–$200, featuring Betaflight FC, long-range RC link, and FPV video transmission.

![License](https://img.shields.io/badge/license-MIT-blue)
![Firmware](https://img.shields.io/badge/Betaflight-4.x-orange)
![Frame](https://img.shields.io/badge/frame-150mm%203inch-lightgrey)
![Status](https://img.shields.io/badge/status-in%20progress-yellow)

---

## Overview

Turbo-FPV is a DIY 5-inch FPV drone project built around the **Simplifly MK5 Pro** digital FPV frame. It uses a Betaflight-compatible F4 V3S flight controller, RS2205 2300KV brushless motors, and streams live FPV video to a **mobile device** — no FPV goggles required. The ExpressLRS RX24T receiver provides a reliable 2.4GHz RC link. Camera and VTX are to be integrated separately.

**Reference build video:** [YouTube — Build Guide](https://www.youtube.com/watch?v=BoLeQCqns5A&t=210s)

**Use case:** FPV freestyle / racing

---

## Hardware

### Parts List

| Component | Model | Price (INR) |
|---|---|---|
| RC Transmitter | FlySky CT6B 2.4GHz 6CH with FS-R6B Receiver | ₹3,464 |
| Flight Controller | F4 V3S Plus — OSD, 2–6S, 3A BEC, 9V Pad, Betaflight | ₹4,011 |
| RC Receiver | ExpressLRS RX24T 2.4G | ₹1,375 |
| Frame | Simplifly MK5 Pro FPV Digital Drone Frame | ₹4,499 |
| Motors | RS2205 2300KV Brushless DC (×2 CW — Black Cap, x2 CCW — Silver Cap ) | ₹2,864 |
| Propellers | Durga Enterprises 5-inch 3-Blade — 2 Pair (CW) | ₹349 |
| Battery | KP 14.8V 1500mAh 65C LiPo (4S) | ₹2,497 |

**Estimated Total: ~₹19,059**

> **Note:** Camera and video transmitter to be sourced separately — see Mobile FPV Streaming Setup below.

---

## Software & Firmware

- **Flight Controller Firmware:** Betaflight 4.x
- **Ground Station / Configurator:** [Betaflight Configurator](https://github.com/betaflight/betaflight-configurator)
- **FPV Streaming (Mobile):** 5.8GHz USB receiver dongle + [Skydroid / Eachine / Skyzone app](https://play.google.com/store) on Android, or a compatible iOS app
- **OS (companion):** N/A (standalone FC, no companion computer)

### Mobile FPV Streaming Setup

The drone uses an analog or digital 5.8GHz VTX (to be sourced). To view the feed on a phone instead of goggles:

1. Get a **5.8GHz USB video receiver dongle** (e.g. Eachine ROTG01 or Skydroid UVC receiver)
2. Plug it into your Android phone via OTG adapter
3. Install **Skydroid** or **Eachine FPV** app
4. Open the app → auto-detects the USB receiver → live feed appears
5. Match the channel on the app to the VTX channel set on your transmitter

> **Note:** This approach works best on Android (full UVC/OTG support). iOS has limited USB OTG support — a workaround is using a WiFi-based receiver module instead.

---

## Installation & Setup

### 1. Flash Betaflight Firmware

1. Download [Betaflight Configurator](https://github.com/betaflight/betaflight-configurator/releases)
2. Connect FC via USB
3. Go to **Firmware Flasher** → select `STM32F405` target
4. Flash latest stable Betaflight release
5. Full chip erase before flashing on a new board

### 2. Configure the FC

```
1. Open Betaflight Configurator → Connect
2. Ports tab: Enable Serial RX on UART where RX is wired
3. Configuration tab:
   - Receiver Mode: Serial (ELRS)
   - ESC/Motor Protocol: DSHOT300 or DSHOT600
   - Enable: Accelerometer, OSD
4. Receiver tab: Verify channel map (AETR1234 for FlySky/ELRS)
5. Motors tab: Test spin direction (no props!)
6. OSD tab: Set up voltage, RSSI, throttle display
```

### 3. Bind the Receiver

```
1. Power the FC with battery (NOT USB alone for bind)
2. Hold bind button on RX24T while powering on
3. Put transmitter in bind mode
4. Solid LED on RX = bound successfully
```

### 4. ESC Calibration

```
1. Remove props
2. Open Betaflight Configurator → Motors tab
3. Enable Motor Test → calibrate ESC endpoints
   OR flash ESCs with BLHeli Suite / BLHeli_32 Suite
```

---

## Configuration

Key Betaflight parameters to set:

| Parameter | Recommended Value | Description |
|---|---|---|
| Motor Protocol | DSHOT300 | For RS2205 ESCs |
| Min Throttle | 1030 | Prevent motor stall |
| Max Throttle | 2000 | Full power cap |
| Failsafe | Drop / Land | Action on RC signal loss |
| Battery Cells | 4S | 14.8V LiPo |
| Low Voltage Cutoff | 3.5V/cell → 14.0V total | Protect battery |

---

## Pre-flight Checklist

- [ ] Props on tight, correct rotation direction (CW/CCW matched to motor positions)
- [ ] Battery fully charged (16.8V = 4.2V/cell for 4S)
- [ ] RC transmitter on, receiver binding confirmed
- [ ] Betaflight Configurator: motors spin in correct direction
- [ ] Failsafe tested — RC off → drone disarms / drops as configured
- [ ] FPV camera feed confirmed on mobile device (USB receiver dongle connected, app open)
- [ ] Video transmitter channel set, no interference
- [ ] Flight area clear, people at safe distance

---

## Usage

### Arm & Fly

| Action | Stick Command |
|---|---|
| Arm | Throttle low + Yaw right (hold 2s) |
| Disarm | Throttle low + Yaw left (hold 2s) |
| Angle mode | Switch SA up |
| Acro mode | Switch SA down |

### Flight Modes (Betaflight)

| Mode | Description |
|---|---|
| Angle | Self-levels, beginner-friendly |
| Acro | Manual, no self-level — full freestyle |
| Air Mode | Keeps motors spinning at zero throttle |

---

## ⚠️ Safety & Warnings

- **Never arm with props on** unless ready for flight outdoors.
- Keep **minimum 10m clearance** from people during testing.
- **4S LiPo safety:** Never discharge below 3.5V/cell. Never leave charging unattended. Use a LiPo-safe bag.
- **Prop strike risk:** 5-inch props at 2300KV can cause serious cuts. Always treat armed drone as dangerous.
- Fly within **visual line of sight** at all times.
- Follow **DGCA (India) regulations** for drone operations. Registration required for drones above 250g.

---

## Troubleshooting

| Issue | Fix |
|---|---|
| Motors not spinning | Check ESC signal wires, re-flash BLHeli, verify motor protocol in Betaflight |
| Drone flips on takeoff | Props on wrong motors, or motor direction reversed — check in Motors tab |
| Video feed with noise | Check VTX antenna, try different channel, move away from 2.4GHz interference |
| No feed on mobile app | Check OTG connection, try different USB cable, ensure app has camera/USB permissions |
| App doesn't detect receiver | Enable OTG in phone settings, try Skydroid or EV-100 app as alternative |
| RC not binding | Re-bind RX24T, check UART and Serial RX settings in Betaflight |
| OSD not showing | Enable OSD in Configuration tab, check camera/FC video wiring |
| Low battery alarm | Set low voltage warning in Betaflight OSD, check LiPo voltage with multimeter |

---

## Roadmap

- [ ] Explore WiFi-based receiver for iOS streaming compatibility
- [ ] GPS module for Return-to-Home
- [ ] Custom PID tune for this motor/prop combo
- [ ] 3D-printed camera mount / FC protection
- [ ] Telemetry logging via Blackbox

---

## Contributing

Pull requests welcome! Please open an issue first to discuss changes.

1. Fork the repo
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit and push, then open a PR

---

## License

MIT — see [LICENSE](LICENSE) for details.
Hardware design files (if any) are shared under CC BY-SA 4.0.
