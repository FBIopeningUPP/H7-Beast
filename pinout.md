# H7-Beast Hardware Pinout Reference

This file is a hardware-oriented summary of the board definition in the ChibiOS
pinout format. It is meant to help with KiCad net naming, connector mapping, and
checking board-level routing.

## MCU / Board

- MCU: STM32H743xx
- Board ID: AP_HW_MATEKH743
- Crystal: 8 MHz external oscillator
- Flash size: 2048 KB
- Bootloader reserve: first 128 KB

## USB / Debug

| Function | MCU Pin | Notes |
| --- | --- | --- |
| OTG_FS_DM | PA11 | USB D- |
| OTG_FS_DP | PA12 | USB D+ |
| SWDIO | PA13 | JTAG/SWD |
| SWCLK | PA14 | JTAG/SWD |

## Motor Outputs

| Motor | MCU Pin | Timer / Channel |
| --- | --- | --- |
| M1 | PB0 | TIM8_CH2N |
| M2 | PB1 | TIM8_CH3N |
| M3 | PA0 | TIM5_CH1 |
| M4 | PA1 | TIM5_CH2 |
| M5 | PA2 | TIM5_CH3 |
| M6 | PA3 | TIM5_CH4 |
| M7 | PD12 | TIM4_CH1 |
| M8 | PD13 | TIM4_CH2 |
| M9 | PD14 | TIM4_CH3 |
| M10 | PD15 | TIM4_CH4 |
| M11 | PE5 | TIM15_CH1 |
| M12 | PE6 | TIM15_CH2 |
| M13 / WS2812 | PA8 | TIM1_CH1 |

## UART / Serial

| Port | TX | RX | Notes |
| --- | --- | --- | --- |
| USART1 | PA9 | PA10 | Telem2 |
| USART2 | PD5 | PD6 | GPS1 |
| USART3 | PD8 | PD9 | GPS2 |
| UART4 | PB9 | PB8 | Spare |
| USART6 | PC6 | PC7 | RC input / SERIAL7 |
| UART7 | PE8 | PE7 | Telem1 |
| UART8 | PE1 | PE0 | Spare |

Extra UART control pins:

- UART7 CTS: PE10
- UART7 RTS: PE9
- USART6 alternate RX option: PC7 on ALT(1)

## SPI Buses

| SPI | SCK | MISO | MOSI | Notes |
| --- | --- | --- | --- | --- |
| SPI1 | PA5 | PA6 | PD7 | IMU1 bus |
| SPI2 | PB15 | PB14 | PB13 | OSD bus |
| SPI3 | PB3 | PB4 | PB5 | External bus |
| SPI4 | PE12 | PE13 | PE14 | IMU2 / IMU3 bus |

Chip select / device pins:

- IMU1_CS: PC15
- MAX7456_CS: PB12
- IMU2_CS: PE11
- IMU3_CS: PC13
- EXT_CS1: PD4
- EXT_CS2: PE2

## IMU / Gyro Devices

| Device | Interface | CS Pin | Notes |
| --- | --- | --- | --- |
| ICM42688 | SPI1 | PC15 | Primary IMU option |
| MPU6000 | SPI1 | PC15 | Alternate IMU option |
| ICM20602 | SPI4 | PE11 | Alternate IMU option |
| ICM42605 | SPI4 | PC13 | Backup IMU option |

## I2C Buses

| I2C | SCL | SDA |
| --- | --- | --- |
| I2C1 | PB6 | PB7 |
| I2C2 | PB10 | PB11 |

## CAN

| Function | MCU Pin | Notes |
| --- | --- | --- |
| CAN1_RX | PD0 | CAN bus RX |
| CAN1_TX | PD1 | CAN bus TX |
| CAN1_SILENT | PD3 | Silent mode control |

## Analog / ADC

| Function | MCU Pin | ADC | Notes |
| --- | --- | --- | --- |
| BATT_VOLTAGE_SENS | PC0 | ADC1 | Battery 1 voltage |
| BATT_CURRENT_SENS | PC1 | ADC1 | Battery 1 current |
| BATT2_VOLTAGE_SENS | PA4 | ADC1 | Battery 2 voltage |
| BATT2_CURRENT_SENS | PA7 | ADC1 | Battery 2 current |
| PRESSURE_SENS | PC4 | ADC1 | Airspeed / pressure |
| RSSI_ADC | PC5 | ADC1 | Analog RSSI |

Defined analog mapping values:

- HAL_BATT_MONITOR_DEFAULT: 4
- HAL_BATT_VOLT_PIN: 10
- HAL_BATT_CURR_PIN: 11
- HAL_BATT2_VOLT_PIN: 18
- HAL_BATT2_CURR_PIN: 7
- HAL_BATT_VOLT_SCALE: 11.0
- HAL_BATT_CURR_SCALE: 40.0
- HAL_BATT2_VOLT_SCALE: 11.0
- HAL_DEFAULT_AIRSPEED_PIN: 4
- BOARD_RSSI_ANA_PIN: 8

## LEDs / Beeper

| Function | MCU Pin | Notes |
| --- | --- | --- |
| LED0 | PE3 | Blue |
| LED1 | PE4 | Green |
| Beeper / ALARM | PA15 | TIM2_CH1 |

GPIO LED assignments:

- HAL_GPIO_A_LED_PIN: 91
- HAL_GPIO_B_LED_PIN: 90
- AP_NOTIFY_GPIO_LED_2_ENABLED: 1

## Storage / SD Card

| Function | MCU Pin |
| --- | --- |
| SDMMC1_D0 | PC8 |
| SDMMC1_D1 | PC9 |
| SDMMC1_D2 | PC10 |
| SDMMC1_D3 | PC11 |
| SDMMC1_CK | PC12 |
| SDMMC1_CMD | PD2 |

Flash storage settings:

- HAL_STORAGE_SIZE: 32768
- STORAGE_FLASH_PAGE: 14

## GPIO / User IO

| Function | MCU Pin | Notes |
| --- | --- | --- |
| PINIO1 | PD10 | Output, LOW default |
| PINIO2 | PD11 | Output, LOW default |

## External / Expansion Notes

- External compass probing is enabled; no built-in compass is defined.
- SPI3 is intended for external peripherals such as PixartFlow on EXT_CS1.
- DMA is configured with `DMA_PRIORITY S*` and `DMA_NOSHARE SPI1* SPI4*` in the board definition.

## Useful KiCad Net Names

When naming connectors or footprints in KiCad, these are the most useful nets to keep aligned with the board definition:

- USB: PA11, PA12
- SWD: PA13, PA14
- IMU1 SPI: PA5, PA6, PD7, PC15
- IMU2 / IMU3 SPI: PE12, PE13, PE14, PE11, PC13
- OSD SPI: PB15, PB14, PB13, PB12
- External SPI3: PB3, PB4, PB5, PD4, PE2
- UARTs: PA9/PA10, PD5/PD6, PD8/PD9, PB9/PB8, PC6/PC7, PE8/PE7, PE1/PE0
- I2C: PB6/PB7, PB10/PB11
- CAN: PD0/PD1/PD3
- ADC: PC0, PC1, PA4, PA7, PC4, PC5