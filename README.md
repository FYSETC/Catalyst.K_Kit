# K1_MAX_MB
The CATALYST motherboard is based on the STM32H723, and the nozzle is based on the STM32G431.

## Flashing for Klipper

### CATALYST

#### USB 

- [*] Enable extra low-level configuration options 
- Micro-controller Architecture (STMicroelectronics STM32)
- Processor model (STM32H723)
- Bootloader offset (No bootloader)
- Clock Reference (25 Mhz crystal)
- Communication interface (USB (on PA11/PA12))
- USB ids (leave default)
- () GPIO pins to set at micro-controller startup

![catalyst-usb](image/catalyst_usb.png)


#### CAN

- [*] Enable extra low-level configuration options 
- Micro-controller Architecture (STMicroelectronics STM32)
- Processor model (STM32H723)
- Bootloader offset (No bootloader)
- Clock Reference (25 Mhz crystal)
- Communication interface (USB to CAN bus bridge (USB on PA11/PA12))
- CAN bus interface (CAN bus (on PD0/PD1))
- (1000000) CAN bus speed
- () GPIO pins to set at micro-controller startup

![catalyst-can](image/catalyst_can.png)

### NOZZLE
#### USB
- [*] Enable extra low-level configuration options 
- Micro-controller Architecture (STMicroelectronics STM32)
- Processor model (STM32G431)
- Bootloader offset (No bootloader)
- Clock Reference (12 Mhz crystal)
- Communication interface (USB (on PA11/PA12))
- USB ids (leave default)
- () GPIO pins to set at micro-controller startup

![zozzle-usb](image/nozzle_usb.png)

#### CAN
- [*] Enable extra low-level configuration options 
- Micro-controller Architecture (STMicroelectronics STM32)
- Processor model (STM32G431)
- Bootloader offset (No bootloader)
- Clock Reference (12 Mhz crystal)
- Communication interface (CAN bus (on PA11/PA12))
- (1000000) CAN bus speed
- () GPIO pins to set at micro-controller startup

![zozzle-can](image/nozzle_can.png)

#### SERIAL
- [*] Enable extra low-level configuration options 
- Micro-controller Architecture (STMicroelectronics STM32)
- Processor model (STM32G431)
- Bootloader offset (No bootloader)
- Clock Reference (12 Mhz crystal)
- Communication interface (Serial (on USART1 PA10/PA9))
- (9600) Baud rate for serial port
- () GPIO pins to set at micro-controller startup

![zozzle-serial](image/nozzle_serial.png)