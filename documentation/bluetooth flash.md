# Bluetooth BLE module configuration

The bluetooth keyboard is based on the Nordic nRF51822 chip and is flashed with the adafruit DFU bluetooth image.  The base module is available from multiple sources but must be flashed with adafruit code as QMK only supports this BLE firmware.

[nRF51822 module](https://www.adafruit.com/product/4076)


## Flashing procedure

### hardware setup
Flashing the nRF51822 module requires a progammer and the adafruit firmware image.

[useful hardware programming setup.](https://bitknitting.wordpress.com/category/ladybug-blue/)
[the hardware programmer I use.](https://www.nordicsemi.com/Products/Development-hardware/nrf51-dk).  There are cheaper options but I was not sure whether I would need to development my own firmware or not, so I went with the native Nordic developer kit.


### toolchain setup
In hindsight this is overkill.  I was preparing to develop the firmware but it turned out to be much easier.

Set up nr51DK SDK 
Setup tool chain per nordicsemi website

Latest version that supports nrf51 series of SOC is v10.0.  Documentation:
https://infocenter.nordicsemi.com/topic/com.nordic.infocenter.sdk51.v10.0.0/index.html
https://infocenter.nordicsemi.com/topic/com.nordic.infocenter.sdk51.v10.0.0/nrf51_getting_started.html

To build nrf51 examples
Update makefile.posix to point to where gcc is installed.  Refer doco above
Go to directory and run Make.

The only comamnd we'll use is nrfprog.


### flashing steps
download [adafruit BLE firmware images](https://github.com/adafruit/Adafruit_BluefruitLE_Firmware)  and save to your local drive.

	1. Bootloader:  `nrfjprog --program /Users/Development/Downloads/Adafruit_BluefruitLE_Firmware-master/bootloader/bootloader_0002.hex -f nrf51 --chiperase`
*Located in the Github files downloaded from Adafruit. for me this is /Users/Development/Downloads/Adafruit_BluefruitLE_Firmware-master/bootloader*
		
	2. S110 soft device:  `nrfjprog --program  /Users/Development/Downloads/Adafruit_BluefruitLE_Firmware-master/softdevice/s110_nrf51_8.0.0_softdevice.hex -f nrf51`
*Located in /Users/Development/Downloads/Adafruit_BluefruitLE_Firmware-master/softdevice/s110_nrf51_8.0.0_softdevice.hex*
		
    3. BLE application: `nrfjprog --program /Users/Development/Downloads/Adafruit_BluefruitLE_Firmware-master/0.7.0/blefriend32/blefriend32_s110_xxac_0_7_0_160628_blefriend32.hex -f nrf51 --sectorerase`


The nRF51822 module can now be soldered to the main keyboard PCB.

*note PCB is designed to allow the nRF51822 module to be program onboard through the JTAG interface, but this requiers another hardware revision.*