# AZSensorSuite

[![License: TAPR-OHL](https://img.shields.io/badge/License-TAPR%20OHL-blue.svg)](https://web.tapr.org/TAPR_Open_Hardware_License_v1.0.txt)

> [!WARNING]
> This device has been mostly tested, however, problems may arise. Expect changes and please report any inaccuracies or issues.


## Overview
<img width="1920" height="1080" alt="land_export" src="https://github.com/user-attachments/assets/4e11e4e2-a3a2-4446-b661-37c5dac60c5e" />

AZSensorSuite (AZSS) is an ultra-low-power, data-acquisition hardware platform designed to be user-programmed for a variety of applications. 

The included example firmware detects when people pass through a doorway into a closed space. A machine learning model (run on a host computer) can then predict when the space is empty so that services like heating or lighting can be automatically disabled.

**Interactive Viewer:** View the PCB and schematic entirely in your browser on [KiCanvas](https://kicanvas.org/?repo=https%3A%2F%2Fgithub.com%2FAZT-GH%2FSensorSuite%2Ftree%2Fmain%2Fhardware).

#

**Special thanks to [**PCBWay**](https://www.pcbway.com/) for sponsoring the development of this project.**

#


### Hardware Features

* **SoC / Connectivity:** Nordic NRF54L15 (via U-Blox Nora B206 module). Supports BLE 6.0, Matter, Thread, Zigbee, and proprietary 2.4GHz communication.
* **ToF Sensor:** ST Microelectronics VL53L4CD. Used for ULP presence detection ([STSW-IMG034](https://www.st.com/en/embedded-software/stsw-img034.html), as low as 154µW) or standard ranging ([STSW-IMG026](https://www.st.com/en/embedded-software/stsw-img026.html)).
* **Environmental Sensor:** Sensirion SHT30 for ambient temperature and humidity readings.
* **Power:** Powered by a CR2032 coin cell, converted to 2.8V. The entire system is ultra-low power (e.g., ~400µA in the human detection example).

> [!TIP]
> The VL53L4CD and SHT30 are both members of pin-compatible sensor families. They can easily be replaced to meet specific project requirements or to navigate supply chain shortages.

## Example Quickstart Guide

### Requirements
* NRF54L15DK (highly recommended) or another SWD debugger.
* SensorSuite PCB and TC2030-IDC-NL cable.
    * *Alternatively:* NRF54L15DK with sensor breakouts.
* [nRF Connect SDK v3.1.1](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/latest/nrf/index.html) via nRF Connect for VS Code.
* Python 3.7 or higher.

### Build and Flash Process
* If working with the development kit, skip step 5 and in `firmware/prj.conf` set `CONFIG_APP_BOARD_IS_PROD=n`.
1.  Clone the repository to a path that contains **no spaces** (e.g., `/projects/sensor_suite` is good; `/projects/sensor suite/` will cause build errors).
2.  Open the application in nRF Connect and navigate to the `firmware` subfolder.
3.  Create a new build configuration:
    * **Target:** `nrf54l15dk/nrf54l15/cpuapp/ns`
    * **Base configuration:** `prj.conf`
    * **Base devicetree overlay:** `boards/nrf54l15dk_nrf54l15_cpuapp_ns.overlay`
    * Select your desired build optimization.
4.  Click **Generate and Build**.
5.  Connect the TC2030-IDC-NL to the DK. The DK will automatically detect it and change the target to the connected custom PCB.
6.  Press **Flash and Erase**. (You may flash without erase after the first flash)
* If a module is being flashed for the first time, it may have memory protections enabled. Click `Yes` in the pop-up to allow recovery.

<img width="462" height="150" alt="error" src="https://github.com/user-attachments/assets/c01f1521-834b-4669-94a2-276189f38c14" />

## PCB Ordering Guide (Sponsored)

Follow these steps to generate fabrication files and order your boards from PCBWay.

### 1. Clone and Open the Project
1. Clone the repository to your local machine.
2. Open `azss.kicad_pro` in **KiCad 9** or newer.
3. *(Optional)* Make any custom modifications needed for your project.

### 2. Run Design Rules Check (DRC)
Run the DRC in KiCad to verify your layout.

> [!NOTE]
> You can safely ignore the following DRC error:
> 
> <img width="303" height="75" alt="KiCad DRC Error" src="https://github.com/user-attachments/assets/adf46d52-0d3a-4314-a157-e372f7856f33" />

### 3. Export Gerber & Drill Files
1. Open the PCB Editor.
2. Go to **File → Fabrication Outputs → Gerbers (.gbr)**.
3. Choose or create a dedicated output directory (e.g., `fabrication/`).
4. Click **Plot**.
5. Click **Generate Drill Files**, keep default options, and click **Generate**.
6. Compress the contents of your output directory into a single **`.zip`** file.

### 4. Upload to PCBWay
1. Go to the [PCBWay Quick Order Page](https://www.pcbway.com/QuickOrderOnline.aspx).
2. Upload your `.zip` file.
3. Inspect your layout using the online **Gerber Viewer** to verify the gerber exported correctly.

### 5. Configure PCB Parameters
Set the board parameters to match the project requirements:

* **Layers:** Select **4 Layers**.

<details>
<summary><b>Click to view 4-Layer Stackup Configuration</b></summary>
<br>
<img width="315" height="213" alt="Layer Stackup Configuration" src="https://github.com/user-attachments/assets/e3a25e56-5cc3-448c-a2f2-9ec223106941" />

</details>

* **Required PCB Specifications:**
  * **Min Track / Spacing:** `4/4mil`
  * **Min Hole Size:** `0.3mm`
  * **Surface Finish:** `Immersion Gold (ENIG)`
  * **Via Process:** `Plugged vias with solder mask`

### 6. Add Assembly / Stencil & Checkout
1. **Stencil / Assembly:** Add an **SMD Stencil** if hand-assembling, or select PCBWay's **Assembly Service** if you want them pre-assembled.
2. Click **Save to Cart**.
3. Wait for PCBWay's order review to complete.
4. Proceed to checkout, pay, and wait for your boards!
   
## Release Notes

v1.0.0 - 17/04/26 - Initial release with all core features implemented


## License

Copyright © 2026 Adam Zembrzuski. 

This project is licensed under the TAPR Open Hardware License ([www.tapr.org/OHL](https://www.tapr.org/OHL)).

A full copy of the license is available in the `license.txt` file in this repository.
