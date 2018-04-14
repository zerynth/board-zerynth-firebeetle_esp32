.. _firebeetle_esp32:

DFRobot FireBeetle Esp32
========================

FireBeetle Esp32 is a low-power consumption development hardware designed for Internet of Things (IoT) by DFRobot. FireBeetle ESP32 integrates a Dual-Core `ESP-WROOM-32 module <https://espressif.com/en/products/hardware/esp32/overview>`_, which supports MCU and Wi-Fi &Bluetooth dual-mode communication.

The main controller supports two power supply methods: USB and 3.7V external lithium battery. And both USB and external DC can charge the Lipo battery directly

.. figure:: /custom/img/firebeetle_esp32.png
   :align: center
   :figwidth: 70% 
   :alt: DFRobot FireBeetle Esp32

Pin Mapping
***********

.. figure:: /custom/img/firebeetle_esp32_pin_io.png
   :align: center
   :figwidth: 100% 
   :alt: DFRobot FireBeetle Esp32

Official reference for DFRobot FireBeetle Esp32 can be found `here <https://www.dfrobot.com/product-1590.html>`_.

Flash Layout
************

The internal flash of the ESP32 module is organized in a single flash area with pages of 4096 bytes each. The flash starts at address 0x00000, but many areas are reserved for Esp32 IDF SDK and Zerynth VM. In particular:

=============  ============  =========================
Start address  Size          Content
=============  ============  =========================
  0x00009000      16Kb         Esp32 NVS area
  0x0000D000       8Kb         Esp32 OTA data
  0x0000F000       4Kb         Esp32 PHY data
  0x00010000       1Mb         Zerynth VM
  0x00110000       1Mb         Zerynth VM (FOTA)
  0x00210000     512Kb         Zerynth Bytecode
  0x00290000     512Kb         Zerynth Bytecode (FOTA)
  0x00310000     512Kb         Free for user storage
  0x00390000       8Kb         Reserved
  0x00392000       4Mb         Free for user storage
=============  ============  =========================

Device Summary
**************

* Microcontroller: Tensilica 32-bit Single-/Dual-core CPU Xtensa LX6
* Operating Voltage: 3.3V
* Input Voltage: 7-12V
* Digital I/O Pins (DIO): 31
* Analog Input Pins (ADC): 4
* Analog Outputs Pins (DAC): 2
* UARTs: 3
* SPIs: 2
* I2Cs: 3
* Flash Memory: 8 MB 
* SRAM: 520 KB
* Clock Speed: 240 Mhz
* Wi-Fi: IEEE 802.11 b/g/n/e/i:

    * Integrated TR switch, balun, LNA, power amplifier and matching network
    * WEP or WPA/WPA2 authentication, or open networks 

Power
*****

Power to the DFRobot FireBeetle ESP32 is supplied via the on-board USB Micro B connector or directly throught the connector for a 3.7/4.2 V battery. The power source is selected automatically.

The device can operate on an external supply of 3 to 5 volts. If using more than 5V, the voltage regulator may overheat and damage the device.

Connect, Register, Virtualize and Program
*****************************************

The DFRobot FireBeetle ESP32 comes with a serial-to-usb chip on board that allows programming and opening the UART of the ESP32 module. Drivers may be needed depending on your system (Mac or Windows) and can be download from `here <https://git.oschina.net/dfrobot/FireBeetle-ESP32/raw/master/FireBeetle-ESP32.inf>`_. In Linux systems, the FireBeetle ESP32 should work out of the box.

.. note:: **For Linux Platform**: to allow the access to serial ports the user needs read/write access to the serial device file. Adding the user to the group, that owns this file, gives the required read/write access:
				
				* **Ubuntu** distribution --> dialout group
				* **Arch Linux** distribution --> uucp group


Once connected on a USB port, if drivers have been correctly installed, the FireBeetle ESP32 device is recognized by Zerynth Studio. The next steps are:

* **Select** the FireBeetle ESP32 on the **Device Management Toolbar** (disambiguate if necessary);
* **Register** the device by clicking the "Z" button from the Zerynth Studio;
* **Create** a Virtual Machine for the device by clicking the "Z" button for the second time;
* **Virtualize** the device by clicking the "Z" button for the third time.

.. note:: No user intervention on the device is required for registration and virtualization process

After virtualization, the FireBeetle ESP32 is ready to be programmed and the  Zerynth scripts **uploaded**. Just **Select** the virtualized device from the "Device Management Toolbar" and **click** the dedicated "upload" button of Zerynth Studio.

Check this video for a live demo:

.. raw:: html

    <div style="margin-top:10px;">
    <iframe width="100%" height="480" src="https://www.youtube.com/embed/EcVGSPHfYFc?ecver=1" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
    </div>


.. note:: No user intervention on the device is required for the uplink process.

Firmware Over the Air update (FOTA)
***********************************

The Firmware Over the Air feature allows to update the device firmware at runtime. Zerynth FOTA in the DFRobot FireBeetle ESP32 device is available for bytecode and VM.

Flash Layout is shown in table below:

=============  ============  ============================
Start address  Size          Content
=============  ============  ============================
  0x00010000       1Mb         Zerynth VM (slot 0)
  0x00110000       1Mb         Zerynth VM (slot 1)
  0x00210000     512Kb         Zerynth Bytecode (slot 0)
  0x00290000     512Kb         Zerynth Bytecode (slot 1)
=============  ============  ============================

For Esp32 based devices, the FOTA process is implemented mostly by using the provided system calls in the IDF framework. The selection of the next VM to be run is therefore a duty of the Espressif bootloader; the bootloader however, does not provide a failsafe mechanism to revert to the previous VM in case the currently selected one fails to start. At the moment this lack of a safety feature can not be circumvented, unless by changing the bootloader. As soon as Espressif relases a new IDF with such feature, we will release updated VMs. 

Secure Firmware
***************

Secure Firmware feature allows to detect and recover from malfunctions and, when supported, to protect the running firmware (e.g. disabling the external access to flash or assigning protected RAM memory to critical parts of the system).

This feature is strongly platform dependent; more information at :ref:`Secure Firmware - ESP32 section<sfw-esp32>`.

Missing features
****************

Not all IDF features have been included in the Esp32 based VMs. In particular the following are missing but will be added in the near future:

    * BLE support
    * Touch detection support 
