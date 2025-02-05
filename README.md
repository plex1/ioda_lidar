# IODA Open Source Lidar

This project implements a time-of-flight lidar from scratch.

Could also serve as a platform for optical communication with pulse position modulation (PPM).

## Main Specification

| Parameter                            |         Value   |  Comment                    |
|------------------------------------- |:---------------:|:----------------------------|
| Resolution distance (single shot)    | 10 cm           | temporal resolution ~700ps  |
| Resolution distance (histogram mode) | <3mm            | temporal resolution <20ps   |
| Resolution angle                     | 0.1125°         | 200 steps per rev, 16 micro steps |
| Pulse Length                         | 1ns             | FWHM                        |
| Pulse Repetition Frequency           | 10MHz           | configurable                |
| Minimum Distance                     | 5cm             |                             |
| Maximum Distance                     | >10m            | white surface               |
| Field of view                        | 360°, 2-axis    | only obstructed by PCB      |

## Components and Links

The system comprised the following modules:

- [ToF PCB (Optical transceiver)](https://github.com/plex1/Tof_PCB)
  - Laser: Infrared 850nm high speed
  - Photo Diode: 2 options - either APD or Si photo multiplier array
  - Microcontroller for configuration and monitoring purposes
    - [Microcontroller software](https://github.com/plex1/TofPCB_SW)
- [Motor PCB](https://github.com/plex1/motor_control_pcb)
  - Microcontroller for profile and step/direction generation
    - [Microcontroller software](https://github.com/plex1/stepper_controller)
  - PWM driver ICs
- [Signal Driver PCB](https://github.com/plex1/ice40_driver_pcb)
  - Drivers and received 50 Ohms high speed signal
  - Variable Delay IC for test and calibration purposes
- [FPGA board (Olimex)](https://www.olimex.com/Products/FPGA/iCE40/iCE40HX8K-EVB/open-source-hardware)
  - Softcore RiscV CPU
  - Tx Sync generation
  - time-to-digital converter implemented with LUTs (between Rx and Tx Sync)
  - histogram measurement
  - [FPGA logicware and software](https://github.com/plex1/SpinalDevTofProject)
- Raspberry Pi 3 Model B
  - For programming, debugging and communication, see [raspice40](https://github.com/plex1/raspice40)
- [Opto-mechanics, 2-axis gimbal](https://github.com/plex1/ioda_gimbal)
  - including focusing and adjustment optics
- [Python Software](https://github.com/plex1/ioda_control_sw)
  - Coordination of gimbal position and time-of-flight data
  - Point cloud generation
 
## Highlights

- compact 2-axis gimbal covering full 3D / 360° field of view, no moving electronics
- time-to-digital converter (TDC) implemented within low cost FPGA including calibration thereof
- Common control and status interface in SW and FPGA modules (Gepin)
- Automated testing environment in Python
- Soft core RiscV in low-cost FPGA ([VexRiscV](https://github.com/SpinalHDL/VexRiscv))
- APD or SiPM photo detector options

## Remarks


- This has been an educational project to learn about: PCB design, RF electronics, FPGA design, CAD design, Optics, microcontrollers and Python
- Design is not done for a product but for testability and modularity (prototype)
- A high power pulsed laser diode and driver designed for lidar would greatly increase range and resolution

## Tools Used

- PCBs: [KiCad](https://www.kicad.org/)
- Mechanics: Autodesk Fusion 360
- FPGA: [SpinalHDL](https://github.com/SpinalHDL/SpinalHDL), [Yosys](https://github.com/YosysHQ/yosys), [NextPnR](https://github.com/YosysHQ/nextpnr)
- Microcontroller: [Arduino](https://www.arduino.cc) and Atmel Visual Studio
- Control and Status Registers: [Cheby](https://gitlab.cern.ch/be-cem-edl/common/cheby)
  
## Block Diagram
![IODA Block Level Block Diagram](./images/ioda_block_diagram_top.png)

# Pictures
![IODA Breadboard](./images/ioda_breadboard.JPG)
![IODA Breadboard](./images/ioda_gimbal.JPG)

# Results
Configuration with OPV310 laser and MTAPD-06-013 photo diode.

2d scans (scanner at origin):

![IODA Breadboard](./images/2d_point_cloud.JPG)
![IODA Breadboard](./images/2d_point_cloud2.PNG)

3d point cloud:

![IODA Breadboard](./images/3d_point_cloud.JPG)

# Further Information
- Hackaday article, incl pdf report: [3D Time-of-Flight Lidar from Scratch](https://hackaday.io/project/197464-3d-time-of-flight-lidar-from-scratch)
- Youtube video: [Gimbal Movement Tests](https://www.youtube.com/watch?v=GpgHjfgOW7Y)

