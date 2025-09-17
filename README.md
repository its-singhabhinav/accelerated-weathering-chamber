# Accelerated Weathering Chamber (AWC) — Product Design (EEL‑48)

> A documentation‑first repository for an Accelerated Weathering Chamber that simulates UV radiation, temperature and humidity to study material degradation. Based on a Ramaiah Institute of Technology product design project (EEL‑48).



---

## 🔎 Overview

The Accelerated Weathering Chamber (AWC) replicates environmental stressors (UV irradiance, temperature, humidity) in a controlled enclosure to accelerate aging tests for materials and coatings. This repo organizes design notes, specs, firmware, and a simple docs site so others can reproduce or extend the build.

> **Why?** Faster, repeatable, standards‑aware testing helps evaluate durability, select materials, and improve product reliability.

---

## ✨ Key Features

- Programmable **day / dark cycles** with independent temperature & humidity set‑points
- **UV exposure** (xenon or UV fluorescent) with adjustable irradiance
- **Closed‑loop control** using sensors and relays for heater / cooler / humidifier
- **Simple HMI** (keypad + display) and optional **IoT telemetry** (ThingSpeak)
- Modular electronics: microcontroller, driver relays, PSU, safety interlocks

> This repo includes a cleaned version of the original Appendix firmware and a reproducible repo layout.

---

## 📐 Specifications (target)

- **Temperature:** −20 °C to 100 °C · control accuracy ±1 °C · uniformity ±2 °C  
- **Humidity:** 10%–95% RH · control accuracy ±3% RH  
- **Light source:** Xenon arc (290–800 nm) or UV‑A 340 nm fluorescent · 0.35–0.80 W/m² @ 340 nm  
- **Cycle programming:** Light/Dark, Temp, RH (e.g., 4 h/4 h)  
- **Safety:** Over‑temperature, humidity overflow, auto‑shutoff

> *Note:* Actual achievable ranges depend on enclosure volume, heater/cooler capacity and lamp choice.

---

## 🗂️ Repository Structure

```
accelerated-weathering-chamber/
├─ docs/                    # GitHub Pages site (Markdown)
│  ├─ index.md              # Project landing page
│  └─ assets/               # Images used in docs
├─ firmware/
│  └─ awc_controller.ino    # Microcontroller firmware (cleaned & commented)
├─ cad/                     # 3D & enclosure models (STEP/SLDPRT/STL)
├─ electronics/
│  ├─ schematics/           # PDF/SCH/KiCad
│  └─ pcb/                  # KiCad project or Gerbers
├─ images/                  # Build, wiring, test captures
├─ report/
│  └─ AWC_Report.pdf        # Original report
├─ LICENSE
└─ README.md
```

---

## 🧰 Bill of Materials (starter)

| Subsystem | Part | Example |
|---|---|---|
| MCU | Arduino/STM32 | Arduino Mega / STM32 Nucleo |
| Temp/RH sensor | DHT22/SHT31 | SHT31 (recommended) |
| UV source | UV‑A 340 nm or Xenon | Q‑lab compatible tubes / xenon lamp |
| Heating | Cartridge / PTC heater | 200–400 W with SSR |
| Cooling | TEC + fan / compressor | Depends on volume |
| Humidification | Ultrasonic / steam | 24 V ultrasonic module |
| Power | 24 V/12 V rails | 24 V/10 A, 12 V/5 A |
| Switching | SSR/Relays | AC SSR 25 A, DC MOSFET boards |
| HMI | TFT or LCD + Keypad | 16×2 LCD or 2.4″ TFT |
| Safety | Fuses, E‑stop, interlocks | Door switch, MOV, earth |

> See `electronics/` for detailed choices and ratings.

---

## 🔌 Firmware

- Language: Arduino (C/C++)  
- Responsibilities:
  - Read temperature/RH sensors
  - Manage **day/dark cycle** timers
  - Control heater/cooler/humidifier and UV lamp via relays
  - Optionally publish telemetry to **ThingSpeak**

Open `firmware/awc_controller.ino` for the full state‑machine implementation and keypad‑driven configuration.

---

## 🌐 Optional IoT (ThingSpeak)

1. Create a ThingSpeak channel with fields: Temp, Humidity, Cycle.
2. Generate API write key.
3. In `firmware/awc_controller.ino`, set `THINGSPEAK_API_KEY` & Wi‑Fi credentials.
4. Build & flash.

---

## 🧪 Operation (typical)

1. Power‑on self‑check; verify door interlock & fans.  
2. Configure:
   - **Day duration** (min)
   - **Dark duration** (min)
   - **Set Temperature** (°C)
   - **Set Humidity** (% RH)
   - **Number of cycles**
3. Start run; controller holds set‑points per phase; telemetry optional.
4. Export run logs and capture images of material specimens.

> Safety: UV radiation & mains voltage are hazardous. Use shielding, interlocks and PPE.

---

## 📊 Results & Notes

- Expect RH to **drop** when temperature rises at constant absolute humidity (inverse relationship visible during warm‑up).
- Validate ranges and uniformity with independent probes before production testing.
- Add DRC/maintenance logs for lamps, filters and seals.

---

## 🗺️ Roadmap / Future Work

- Web HMI (local dashboard), on‑device datalogging (SD)
- Irradiance sensor feedback for closed‑loop UV control
- Cloud database (Firebase) and remote alerts
- Additional stressors: rain/spray, thermal shock, electrical stress

---

## 📄 License & Attribution

Choose a license (e.g., MIT) and place it in `LICENSE`.  
This repository’s documentation is based on an academic project report; credit the original authors and institution in **Acknowledgements**.

---

## 🙌 Acknowledgements

- Ramaiah Institute of Technology — Product Design (EEL‑48)
- Original student authors & faculty coordinators (listed in report)
- Standards: ASTM G154 / ISO 4892 family

