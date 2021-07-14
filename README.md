# PS_Fgen_HW
Hardware for an PowerSupply and Functiongenerator build from an ATX power supply.

![PS_Fgen_HW](/Images/HW_complete.jpg)

## ATX Supply
The hardware of the power supply and function generator is based on the following ATX supply:
``Yuan Kee 230P ATX``

![ATX](/Images/HW_ATX_Label.jpg)

By using another ATX supply, some output capabilities can be changed / enhanced.

## Input / Output Capabilities

![Connectors](/Images/HW_Connectors_near.jpg)

The power supply and function generator features the following capabilites:

### Power Supply output (PS+)
One power supply output is available.
Connector		| PS+
-----------------------	| -----------------
Voltage			| 0 .. 10 V DC
Current			| 0 .. 2 A
Protections		| OVP, OCP, OPP

The voltage and current regulation is done in firmware.
The protections are implemented in firmware, too.

### DDS outputs (DDS1, DDS2)
Two direct digital synthesis outputs are available.
Connector		| DDS1						| DDS2
-----------------------	| ---------------------------------------------	| ---------------------------------------------
Amplitude		| 0 .. 20 Vpp					| 0 .. 20 Vpp
Offset			| -10 V .. 10 V					| -10 V .. 10 V
Frequency		| approx. max. 170 kHz				| approx. max. 170 kHz
Waveforms		| FW defined (Sine, Square, Triangle, ...)	| FW defined (Sine, Square, Triangle, ...)

The waveforms are defined in firmware. Default waveforms like sine, square and triangle are supported. By using DDS it is possible to even generate user defined waveforms.
The maximum frequency is limited by the low pass filter after the DAC outputs. The cutoff frequency is approximately 170 kHz. The real frequency range depends on the DDS implementation in the firmware!

### DMM inputs (DMM1, DMM2)
Two digital multimeter inputs are available.
Connector	| DMM1		| DMM2
---------------	| -------------	| ---------------
Input Range	| 0 .. 20 V DC	| 0 .. 20 V DC

**Caution:** Negative voltages on the DMM inputs are not allowed!

### USB outputs
Two USB outputs are available. They are connected to the 5V output of the ATX supply. So the combined current capability for both USB ports are 23 A.

### USB input
The USB input can be used to communicate to the device via serial port.

### ATX outputs
All output voltages of the ATX supply are directly available through connectors on the connector panel. The currents are directly depending on the used ATX supply (values for the Yuan Kee 230P ATX).
Connector		| +5V	| +3V3	| +12V	| -12V
-----------------------	| -----	| -----	| -----	| -----
Voltage			| 5 V	| 3,3 V	| 12 V	| -12 V
Current (max.)		| 23 A	| 14 A	| 9 A	| 0.8 A

## User Interface

![FrontPanel](/Images/HW_FrontPanel.jpg)

The user interface is made up of the following components:
- Graphical display: A 240x64 pixel graphical LCD is used to display the device status and current settings.
- Encoder: A rotary encoder can be used for value editing and menu navigation.
- KeyPad: A 16 button keypad can be used for value editing and menu navigation.

The user interface logic is defined by the firmware.

## Electronic
The electronic is divided into two PCBs:
- **FrontPanel:** The front panel is containing 16 push buttons forming a 4x4 keypad and a rotary encoder for user input. It is connected to the MainBoard using a ribbon cable.
- **MainBoard:** The main board is containing all other components that can be grouped like following:
	- *Microcontroller*: The main controller of the power supply and function generator. It controls everything from user input handling to output generation. The used type is an ATmega1284P.
	- *Serial communication:* A FT232RL USB-UART IC is used for serial communication to the microcontroller via USB.
	- *GLCD:* The used graphic LCD model is ``Varitronix, COG-VLGEM1277-01``. It has 240x64 pixel and is controllable via SPI. The onboard display driver is ``S1D15721`` **Important:** The display uses a 3.3 V logic level!
	- *ATX voltage monitors:* The ATX 5 V and 3.3 V lines are each directly connected to ADCs of the microcontroller. The 12 V line is using a resistor voltage divider to scale the voltage to the allowed input range of 0..5V of the ADC inputs. The -12 V line is also using a OpAmp to negate the voltage of the resistor divider to be within the 0..5V range of the ADC inputs.
	- *DMM inputs:* The two DMM inputs can be used to measure positive input voltages. The resistor divider scales the input voltage so that the maximum DMM input voltage equals 5 V (maximum allowed voltage of the microcontroller ADC inputs).  **Important:** Negative voltages can't be measured with the DMM inputs because the ADC inputs are damages by negative voltages. Also voltages higher than the maximum DMM input voltage can damage the inputs.
	- *DDS outputs:* The two DDS output signals are generated by a dual output DAC IC (MCP4921, output 0..5 V for each output) followed by an offset and amplification stage (used to create a range of -10 V .. 10 V). The output is buffered by an OpAmp and two transistors for higher currents.
	- *PS output:* The PS output signal is generated by a single output DAC IC (MCP4921, output 0..5 V) and buffered by an OpAmp and two transistors for higher currents. The voltage at the PS output is measured by a simple resistor voltage divider. The current at the PS output is measured by transforming it into a voltage proportional to the current.

The FrontPanel PCB was designed to be manufactured with toner transfer method.
The MainBoard PCB was too complex for manual manufacturing and was therefore made by JLCPCB. 

![MainBoard](/Images/HW_MainBoard.jpg)

## Housing
All housing components are made for the above ATX supply and are not guaranteed to fit for any other ATX supply. Nevertheless all construction files are available in the Mechanical folder and can be adapted, if neccessary.
The parts were designed using Autodesk Fusion 360 (.f3d files) and are exported as .stl files.
The .stl files were used to 3D print all components.
The following parts are needed:
Quantity	| File name	
---------------	| ----------------------------
1 		| Abdeckung_hinten.stl
1		| Encoder_Knopf.stl
1		| FrontPanel_Rahmen.stl
1		| Griff.stl
1		| Plaettchen.stl
1		| Seitenteil_links.stl together with Schrift_Seitenteil_links.stl
1		| Seitenteil_links_Schublade.stl
1		| Seitenteil_rechts.stl together with Schrift_Seitenteil_rechts.stl
1		| Taste_k.stl
1		| Taste_komma.stl
1		| Taste_links.stl
1		| Taste_m.stl
1		| Taste_minus.stl
1		| Taste_rechts.stl
1		| Taste0.stl
1		| Taste1.stl
1		| Taste2.stl
1		| Taste3.stl
1		| Taste4.stl
1		| Taste5.stl
1		| Taste6.stl
1		| Taste7.stl
1		| Taste8.stl
1		| Taste9.stl

## Firmware
The firmware for this power supply and function generator is developed in another repository:
[GitHub - M1S2/PS_Fgen_FW: Firmware for an Power Supply and Function Generator build from an ATX power supply](https://github.com/M1S2/PS_Fgen_FW)