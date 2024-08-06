# IODA Lidar

This project implemnts a time-of-flight lidar from scratch.

## Main Specification

| Parameter                            |         Value   |  Comment                    |
|------------------------------------- |:---------------:|:----------------------------|
| Resultion distance (signle shot)     | 10 cm           | temporal resolution ~700ps  |
| Resultion distance (histogram mode)  | <3mm            | temporal resolution <20ps   |
| Resolution angle                     | 0.1125°         | 200 steps per rev, 16 micro steps |
| Pulse Length                         | 1ns             | FWHM                        |
| Pulse Repeptition Frequency          | 10MHz           | configurable                |
| Minimum Distance                     | 5cm             |                             |
| Maximum Distance                     | >10m            | white surface               |
| Field of view                        | 360° 2-axis     | only obstructed by PCB      |

## Components and Links

The system comprised the following modules:

- [Optical Transceiver PCB (Tof)](https://github.com/plex1/Tof_PCB)
  - Laser: Infrared 850nm high speed
  - Photo Diode: 2 options - either APD or Si photo multiplier array
  - Microcontroller for configuration and monitoring purposes
    - [Microcontroller software](https://github.com/plex1/TofPCB_SW)
- [Motor PCB](https://github.com/plex1/motor_control_pcb)
  - Microcontroller for profile and step/direction generation
    - [Microcontroller software](https://github.com/plex1/stepper)
  - PWM driver ICs
- [Signal Diver PCB](https://github.com/plex1/ice40_driver_pcb)
  - Drivers and recieved 50 Ohms high speed signal
  - Variable Delay IC for test and calibration purposes
- [FPGA board (Olimex)](https://www.olimex.com/Products/FPGA/iCE40/iCE40HX8K-EVB/open-source-hardware)
  - Tx Sync generation
  - time to digital converter implemented with LUTs (between Rx and Tx Sync)
  - histogram measurement
  - [FPGA logicware](https://github.com/plex1/SpinalDevTof)
- Raspberry Pi 3 Model B
  - For programming and debugging
- 2-axis Gimbal
  - indcluding focusing and ajustement optics
- [Python Software](https://github.com/plex1/ioda_control_sw)
  - Coordination of gimbal positio and time-of-flight data
  - Point cloud generation
 
## Highlights

- 2-axis gimbal covering full 360 degrees field of view
- time-to-digital converter implemented within low cost FPGA including calibration thereof
- Common control and status interface between SW and FPGA modules (Gepin)
- Soft core RiscV in low-cost FPGA
- APD or SiPM detector options

## Tools Used

- PCBs: [KiCad](https://www.kicad.org/)
- Mechanics: Autodesk Fusion 360
- FPGA: [Yosys](https://github.com/YosysHQ/yosys) and [NextPnR](https://github.com/YosysHQ/nextpnr)
- Microcontroller: [Arduino](https://www.arduino.cc) and Atmel Visual Studio
- Control and Status Registers: [Cheby](https://gitlab.cern.ch/be-cem-edl/common/cheby)
  
## Block Diagram
![IODA Block Level Block Diagram](./images/ioda_block_diagram_top.png)

# Some Images
![IODA Breadboard](./images/ioda_breadboard.JPG)
![IODA Breadboard](./images/ioda_gimbal.JPG)
