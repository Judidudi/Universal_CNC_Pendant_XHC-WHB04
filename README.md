# Universal_CNC_Pendant_XHC-WHB04
This Repository contains Software and Hardware for a complete CNC-Pendant which emulates the protocol of the original XHC-WHB04 Pendant. This Pendant works out of the Box with Linux-CNC, Beacon, Mach3 and Mach4.


<!-- Logo: replace with your final mark -->
<p align="center">
  <img src="docs/images/logo.svg" alt="gleeworks" width="120">
</p>

<p align="center">
  <img src="docs/images/explosion.gif" alt="Exploded view of the pendant" width="700">
</p>

---

## Features

- **Drop-in WHB04B-6 emulation** – recognized by LinuxCNC's stock `xhc-whb04b-6` driver
- **60 mm handwheel**, rotary switches for axis selection and feed override
- **8 mechanical keys** (Kailh Choc V2) with configurable mapping
- **2.8" TFT touch display** (LVGL) showing machine position and status
- **Hardwired E-stop** – routed as a physical contact to the machine, completely independent of USB
- **Connection watchdog** – the pendant detects a lost host connection and can trigger the E-stop

## Compatibility

| Controller | Status |
|------------|--------|
| LinuxCNC   | To be tested |
| Beamicon2  | WIP |
| Mach 3     | WIP |
| Mach 4.    | WIP |

## Repository structure

```
firmware/     Pico 2 W firmware (C, Pico SDK, TinyUSB, LVGL)
hardware/
  ├─ pcb/         KiCad projects (main board, E-stop breakout)
  ├─ enclosure/   → printable parts are hosted on Printables (see below)
  └─ bom/         Bill of materials
docs/         Assembly guide, wiring, images
```

### Enclosure

All printable parts (3MF/STL) are available on **[Printables](https://www.printables.com/)** <!-- TODO: link -->.
Check [`hardware/enclosure/README.md`](hardware/enclosure/README.md) for which enclosure version matches which PCB revision.

## Getting started

### 1. Clone

LVGL is included as a Git submodule, so clone recursively:

```bash
git clone --recursive https://github.com/gleeworks/cnc-pendant.git
```

Already cloned without `--recursive`? Run:

```bash
git submodule update --init
```

### 2. Build the firmware

Requires the [Raspberry Pi Pico SDK](https://github.com/raspberrypi/pico-sdk) with `PICO_SDK_PATH` set
(or the Raspberry Pi Pico extension for VS Code).

```bash
cd firmware
mkdir build && cd build
cmake -DPICO_BOARD=pico2_w ..
make
```

### 3. Flash

Hold **BOOTSEL** while plugging in the Pico, then copy the generated `.uf2` file to the `RP2350` drive.
Prebuilt `.uf2` files are attached to each [release](../../releases).

### 4. Set up LinuxCNC

See [`docs/linuxcnc-setup.md`](docs/linuxcnc-setup.md). <!-- TODO -->

## Building your own

The step-by-step guide is in [`docs/assembly-guide.md`](docs/assembly-guide.md). <!-- TODO -->

## Safety

- The **RESET** key toggles machine on/off – it is **not** an emergency stop.
- The pendant's E-stop only protects you if it is wired into your machine's **hardware safety chain**. Never rely on a software-only E-stop.
- This is a hobby project. Build and use it at your own risk.

## Third-party License

| Component | License |
|-----------|---------|
| [LVGL](https://github.com/lvgl/lvgl) v8.3 | MIT |
| [TinyUSB](https://github.com/hathach/tinyusb) (via Pico SDK) | MIT |
| [Raspberry Pi Pico SDK](https://github.com/raspberrypi/pico-sdk) | BSD-3-Clause |

Full license texts: [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md)

## Acknowledgements

- The LinuxCNC project and the authors of the `xhc-whb04b-6` component
- The LVGL and TinyUSB communities
