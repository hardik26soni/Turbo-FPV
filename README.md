# 🚁 Turbo-FPV

A 5-inch FPV racing drone I'm building from scratch. Total cost is around ₹19,059. No goggles — video streams straight to my phone.

![License](https://img.shields.io/badge/license-MIT-blue)
![Firmware](https://img.shields.io/badge/Betaflight-4.x-orange)
![Status](https://img.shields.io/badge/status-in%20progress-yellow)

---

## What is this?

Turbo-FPV is my DIY FPV drone build based on the Simplifly MK5 Pro frame. I'm using a Betaflight F4 V3S FC, RS2205 2300KV motors, and an ExpressLRS receiver for the RC link. The plan is to stream the FPV feed to my phone instead of using goggles.

Build reference: [YouTube](https://www.youtube.com/watch?v=BoLeQCqns5A&t=210s)

---

## Parts

| Component | Model | Price (INR) |
| RC Transmitter | FlySky CT6B 2.4GHz 6CH with FS-R6B Receiver | ₹3,464 |
| Flight Controller | F4 V3S Plus — OSD, 2–6S, 3A BEC, 9V Pad, Betaflight | ₹4,011 |
| RC Receiver | ExpressLRS RX24T 2.4G | ₹1,375 |
| Frame | Simplifly MK5 Pro FPV Digital Drone Frame | ₹4,499 |
| Motors | RS2205 2300KV (×2 CW Black Cap + ×2 CCW Silver Cap) | ₹2,864 |
| Propellers | Durga Enterprises 5-inch 3-Blade — 2 Pair (CW + CCW) | ₹349 |
| Battery | KP 14.8V 1500mAh 65C LiPo (4S) | ₹2,497 |

**Total: ~₹19,059**

---

## Getting it flying

### 1. Flash Betaflight
```
- Download [Betaflight Configurator](https://github.com/betaflight/betaflight-configurator/releases)
- Connect FC via USB, go to Firmware Flasher
- Target: `STM32F405` — do a full chip erase first on a new board
- Flash latest stable release
```

### 2. Configure the FC

```
1. Ports tab → Enable Serial RX on the UART your receiver is wired to
2. Configuration tab:
   - Receiver Mode: Serial (ELRS)
   - ESC/Motor Protocol: DSHOT300 or DSHOT600
   - Enable: Accelerometer, OSD
3. Receiver tab → verify channel map (AETR1234)
4. Motors tab → test spin direction with no props!
5. OSD tab → set up voltage, RSSI, throttle
```

### 3. Bind the receiver

```
1. Power the FC from battery (USB alone won't work for binding)
2. Hold bind button on RX24T while powering on
3. Put transmitter in bind mode
4. Solid LED on RX = you're bound
```

### 4. Calibrate ESCs

```
1. Props off
2. Betaflight → Motors tab → Enable Motor Test
3. Calibrate endpoints, or flash ESCs via BLHeli Suite
```

### 5. Stream FPV to phone

Since I'm not using goggles, the FPV feed goes to my Android phone:

1. Pick up a **5.8GHz USB receiver dongle** (Eachine ROTG01 works well)
2. Plug into phone via OTG adapter
3. Install **Skydroid** or **Eachine FPV** app
4. Open app → it auto-detects the dongle → live feed shows up
5. Match the channel in the app to whatever channel your VTX is on

> iOS note: OTG support on iPhone is patchy. A WiFi-based receiver module is a better bet if you're on iOS.

---

## Betaflight params to set

| Parameter | Value | Why |
| Motor Protocol | DSHOT300 | Works well with RS2205 ESCs |
| Min Throttle | 1030 | Stops motors from stalling |
| Max Throttle | 2000 | Full power cap |
| Failsafe | Drop / Land | What happens when RC signal dies |
| Battery Cells | 4S | 14.8V pack |
| Low Voltage Cutoff | 3.5V/cell (14.0V total) | Don't kill the battery |

---

## Before every flight

- [ ] Props tight, CW/CCW matched to the right motors
- [ ] Battery at full charge (16.8V for 4S)
- [ ] Transmitter on, receiver linked
- [ ] Motors spinning the right way in Betaflight (no props)
- [ ] Failsafe tested
- [ ] FPV feed showing on phone
- [ ] Area clear of people

---

## Arming and flight modes

| Action | Stick input |
| Arm | Throttle low + Yaw right (hold 2s) |
| Disarm | Throttle low + Yaw left (hold 2s) |
| Angle mode | Switch SA up |
| Acro mode | Switch SA down |

| Mode | What it does |
| Angle | Self-levels — good for learning |
| Acro | Full manual, no self-level |
| Air Mode | Motors stay spinning at zero throttle |

---

## ⚠️ Safety

- Don't arm with props on unless you're actually about to fly.
- Keep at least 10 metres from people when testing.
- Never leave a LiPo charging unattended. Use a LiPo bag.
- Never drain below 3.5V/cell — it kills the battery permanently.
- These props at 2300KV will cut you. Treat an armed drone like it's always about to spin up.
- Fly within line of sight.
- DGCA rules apply in India — drones above 250g need to be registered.

---

## Common issues

| Problem | Fix |
| Motors not spinning | Check ESC signal wires, re-flash BLHeli, check motor protocol in Betaflight |
| Flips on takeoff | Props on wrong motors or motor direction wrong — fix in Motors tab |
| Noisy video feed | Check VTX antenna, switch channels, move away from 2.4GHz sources |
| No feed on phone | Check OTG cable, give the app camera/USB permissions |
| App not detecting dongle | Enable OTG in phone settings, try Skydroid or EV-100 app |
| RC not binding | Re-bind RX24T, check UART and Serial RX config in Betaflight |
| OSD not showing | Enable OSD in Configuration tab, check camera wiring to FC |
| Low battery alarm | Set warning in Betaflight OSD, verify voltage with multimeter |

---

## Contributing

Open an issue before submitting a PR. Branch off main, make your changes, open the PR.

---

## License

MIT. Any hardware files are CC BY-SA 4.0.
