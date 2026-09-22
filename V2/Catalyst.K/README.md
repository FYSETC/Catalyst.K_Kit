# Catalyst.K
# Introduction
Catalyst.K is the main circuit board installed at the bottom of the machine. For connection instructions, please refer to the kit connection diagram.

# Firmware compilation and upload
## Firmware Configuration
### General Klipper
#### Serial and USB
<p align="left">
  <img src="./Image/k1_serialusb.png" width="560" height="140">
</p>

#### Can
<p align="left">
  <img src="./Image/k1_can.png" width="560" height="140">
</p>

### K1_Series_Klipper
#### Serial and USB
<p align="left">
  <img src="./Image/k1_creality_serialusb1.png" width="560" height="160">
</p>

<p align="left">
  <img src="./Image/k1_creality_serialusb2.png" width="560" height="120">
</p>

#### Can
<p align="left">
  <img src="./Image/k1_creality_can.png" width="560" height="180">
</p>

# Firmware Upload
Use the following commands to compile and upload the firmware.
```
cd ~/klipper
make flash FLASH_DEVICE=0483:df11
```