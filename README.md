## Microclimate control system as diploma work

I will make short review how I made __COMPACT ELECTRONIC MICROCLIMATE MAINTENANCE SYSTEM__, what decisions was good or bad. And how I would do it now.
It is really my Bachelor work, that I made on Arduino Nano. Diploma work was devided on 3 parts. 25 out of 83 pages was assigned to theory. Developing of system covered  33 pages.

## Table of Contents
1. [Microclimate](#Microclimate)
    1. The main indicators of the microclimate
    2. The appropriate sensor that will measure and actuator responsible for changing these parameter
    3. OFFICE, HOUSING parameters of the microclimate
    4. Ranges of parameters
2. [Arduino platform](#The-Arduino-platform)
3. [Development of a microclimate control system](#Development-of-a-microclimate-control-system)
    1. [Development of a functional diagram of the device](#Development-of-a-functional-diagram-of-the-device)
    2. [Program code and its debugging](#Program-code-and-its-debugging)
    3. [Creating of PCB for the device](#Creating-of-PCB-for-the-device)
    4. [Stages in creating a printed circuit board](#Stages-in-creating-a-printed-circuit-board)
    5. [Creating a compact greenhouse box](#Creating-a-compact-greenhouse-box)
4. [Conclusions](#Conclusions)
    1. What choices was good
    2. What decisions was bad
    3. How I would do it now


## Microclimate
Today, with the growth of technological progress, changing environmental conditions is an inevitable process.
To ensure the most efficient working capacity of employees, employers create the necessary conditions for this. The microclimate is one of the main factors. And for plants, agronomists maintain their microclimate to get the best harvest and ensure their growth.

**The main indicators of the microclimate:**
- Air temperature
- Air humidity
- Atmospheric gas composition
- Light
- Atmospheric pressure
- Soil moisture

The microclimate parameters will be measured by sensors, which can be divided into digital and analog. In the case of a digital sensor, the result can, without any transformations, be output in the parameter values (%, ° C (). When using an analog sensor, it will be necessary to use a certain function to convert the received signal or to start directly from the analog value (0… 1023).
The obtained values will be processed by a microcontroller (MC), which in turn will compare the received signals and adjust them through the executive subsystems. Accordingly, a signal will be generated at a certain output of the MC, which will further affect the required actuator (actuator).

**The appropriate sensor that will measure and actuator responsible for changing these parameters:**
1.  Temperature sensor - thermocouple, electric heater, gas boiler…;
2. Humidity sensor - ultrasonic water spray;
3. Gas composition sensor - ventilation system, window ventilation;
4. Light sensor - lamps, bulbs, LED strips;
5. Soil moisture sensor - water pump.

**Next, consider the option for OFFICE, HOUSING maintenance of the microclimate:**
1. Air temperature;
2. Humidity;
3. Carbon dioxide content.

The rationality of the use of actuators depends on the size of the room, as well as the environment. So to control the temperature in the greenhouse box (compact greenhouse size 50x50x50cm), we can use the Peltier element, air heaters or even some lamps. This box can be powered by 12V and even batteries. Due to its small size, the creation of this greenhouse box greatly simplifies the implementation of this device.
But to heat the cottage you need to use a gas or electric boiler, which is controlled by a voltage of 220V.
Actuators will be controlled mainly by an **electromagnetic relay**, which will further control the required load, and in special cases, for example, you need to smoothly change the brightness of the lamps, it can be a **field-effect transistor**.

**Let's define necessary ranges of parameters of a microclimate for a choice of this or that sensor:**
- Temperature: from 5 to 40 ° C;
- Humidity: from 20 to 80%;
- Lighting: has no special requirements (from 10 to 100 points);
- Soil moisture: no special requirements (from 10 to 100 points).

When choosing an actuator, you must pay attention to its power so that it can maintain the parameters in the range necessary for us.

## The Arduino platform
Arduino-based devices can receive environmental information through external sensors, and in response to the obtained sensor values, they can control various actuators.
The microcontroller on the board is programmed using a special programming language, which is a simplified version of C ++ / C. The USB connector is used for programming without the use of special programmers. Direct programming takes place in the Arduino IDE. You can also program the platform using other programs, such as: C ++, Python, Java.
There is currently a wide variety of Arduino platforms. The most popular are **Arduino Uno** (for its wide functionality) and **Arduino Nano** (for its compactness and price).
Arduino Nano is one of the smallest Arduino boards. It is a complete analogue of the Arduino Uno - it also works on the ATmega328P chip (although you can still find variants with the ATmega168), but with a smaller form factor. Due to its overall size, the board is often used in projects where compactness is important. There is no separate external power socket on the board, the Arduino works via USB (miniUSB or microUSB). Otherwise, the parameters are the same as the Arduino Uno model.
The board contains from six pins of analog inputs and fourteen or more pins of digital inputs and outputs, for connection to the microcontroller of necessary electronic devices and schemes.

## Development of a microclimate control system
A microclimate control system will be developed in the **greenhouse box**, as its small size and price of components allow you to assemble this device yourself. But nothing prevents to replace actuators with more serious ones, which work from the 220V network. They will be controlled by a relay or transistor switch, which will change its state (open / closed) from the value at the output of the logic pin of Arduino.
Let's develop the functional and electric scheme of the device, and also we will collect it on a breadboard.
### Development of a functional diagram of the device

![functional diagram](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/func.png?raw=true)

`I was lazy to translate it ;D`

The functional diagram shows all the components that are responsible for removing and changing the microclimate, control and display of data. Also on the diagram there are potentially possible devices that are displayed by a dashed line.
Let's take a closer look at the relationship between each block of the circuit and its connection. For full operation of actuators it is necessary to use a power part, such as a relay or a transistor key. Since the **output of the microcontroller can emit 20 mA** - this is not enough to power the actuators, the maximum for the LED. Therefore, the power part is used, where an external voltage source is connected, both 5 V and 220 V. The advantage of the relay is the so-called galvanic isolation, where the power part is not connected to the logic, in contrast to the transistor switch. But sufficient current is required to power the coil in the relay. But in the case of a transistor, its opening is much less current, and it allows you to control the output voltage by changing it in the range from zero to maximum at the input. Thus, the transistor switch is convenient to use for the lighting mode "Dawn", where the brightness of the lamp increases over time, by analogy with sunrise and sunset.
To power the modules, it is desirable to use an** external voltage source**, which is mainly 5 V. This will save the service life of the MC.
Sensors and actuators will be connected to digital and analog pins, depending on the type of module. The display and the time module will be connected via the **I2C interface**, A4 and A5 pins. Up to 128 modules can be connected to this interface. And the data transfer rate reaches 100 kbit / s, let me remind you, it's only two pins of the microcontroller. 
To work with the Wi-Fi module, you will need to download a special program to your smartphone, which will send commands to the MC via IP address.
The camera, IR sensor and SD module are connected to the digital inputs.
Mainly written **libraries** will be used to communicate with the modules, which will greatly simplify the work of receiving and sending to the modules.
To select certain circuit modules, we will start from the already existing components. Thus, the **DHT11 module **will be responsible for  the humidity and temperature, which will save 1 analog input, as 1 digital pin is used to transmit temperature and humidity data. As a light sensor we use a budget** photoresistor and a resistor** connected as a voltage divider, 10 kОhm. And for the **soil moisture sensor**, 2 nails and a similar resistor will be used as for the light sensor.

**Let's consider in more detail the most important modules in this scheme, with the exception of the above sensors and actuators:**
- **Relay** - a switching device that connects or disconnects the circuit of the electrical circuit when changing the input voltage (current). Relays are used
where you want to control an electrical circuit with a low power signal and full galvanic isolation, or where several circuits must be controlled by a single signal.
- **Infrared receiver and remote control.** The operation of the remote control is as follows. When the button is pressed, the signal is encoded in infrared light, the receiver receives it and performs the necessary actions. The information is encoded in the form of a logical sequence of pulse packets with a certain frequency.
- **The DS3231** - time module is a board with accurate real-time clocks. The module will be used for lighting control and data recording.
- ** LCD1602** - liquid crystal character LCD display. Designed to display textual information. The display allows you to display up to 32 characters at a time (2 lines of 16 characters). The screen is equipped with a rear, the brightness of which can be controlled.

### Program code and its debugging

After creating the electrical circuit, it had to be assembled on a test bench and connect the necessary sensors, which are necessary when working with the code. When writing the code (sketch), first of all, it was necessary to establish a connection with the Arduino Nano. LEDs were used as actuators, which clearly indicated which parameter was changing. Owning the breadboard made my life easier.
![pic](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/breadb.png?raw=true)

[Screens](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/screen.png?raw=true)


### Creating of PCB for the device
The printed circuit board is a special plate on which the electrically conductive circuits of the electronic circuit are placed. This figure is placed on a dielectric substrate, a material that does not conduct current.

When I first designed and diluted the tracks, I tried to place all the components in the area, which was very difficult. After making the board and installing the components, it turned out that some functions did not work, and completely connecting the components was not convenient. Therefore, it was decided to design a new board, which will house the logic part (without relays). So I designed 2 PBC and out of two made one.

![pic](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/Schematics.png?raw=true)

[PCB](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/pcb1.png?raw=true)

### Stages in creating a printed circuit board
![pic](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/steps.png?raw=true)

Hovever several versions of the boards have been implemented, the drawings of both versions will be used. In general, the process of making a printed circuit board can be divided into the **following stages**:
- **Forming a photo template**;
- **Applying a photoresist** on a printed circuit board;
- **Transfer** of a photo template on a textolite - we paste a photo template on a photoresist;
- **Exposure** - the board on which the photoresist is applied must be exposed to ultraviolet radiation through a photomask, using a UV setting.
- **Vanishing** – is the process of removing a photoresist that was not illuminated (under a toner layer). After thorough rinsing under running water, you can proceed to the next step;
- **Digestion** - iron chloride is usually used as a digestion solution. Iron chloride corrodes any metal, including stainless steel. Since it is a very harmful substance, it was decided to use a solution of 100 ml. hydrogen peroxide, 30 g of citric acid and 5 g of salt.
- Final **removal of the photoresist**;
- **Soldering mask application** - the mask is designed to protect the PCB from the environment. Applying a one-component mask is by silk-screen printing or fleece film;
- **Creating a photo template for holes** - in the environment you can choose to export images of holes and soldering areas;
-** Transferring** a photo template to textolite is a similar process;
- **Exposure** - a similar process;
-** Removal of residual mask** - performed with a cotton swab and acetone;
- **Tanning** is the final curing of the mask, which is performed in the oven for 15-30 minutes;
-** Hole drilling** - 1 and 2 mm holes were drilled with a special tool. This step must be performed with special meticulousness, because the slightest shift can damage the tracks or this hole will not be a certain component;
-** Site tinning** - this stage was performed by direct mounting of elements on the printed circuit board
- **Trimming excess textolite** - in my case, this operation was not performed because there is no suitable tool. This stage performs an aesthetic function if there are no requirements for the size of the device.

![pic](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/ready.png?raw=true)

### Creating a compact greenhouse box

To complete this project, this box was developed, which will be made of 2 mm thick glass. (baguette), dimensions 266x248x156 mm (WxDxH). It is based on a pentagon with sides of 100 mm, and the sides measuring 210x100 mm.

![pic](https://github.com/dDenVil/Microclimate_control_system_as_diploma_work/blob/main/readme_assets/greenbox.png?raw=true)

On one side there will be a hole for a microclimate control device. The glass parts will be joined with silicone, which will ensure the tightness and aesthetics of this product. A glass lid will also open for easy access. The result was obtained without a cover, because there was no necessary fastener.

## Conclusions

This was very fast review, I didn't even had to translate it, because I did for my Enlish lessons later.    

**Let's have a look what choices was good:**
- It was working;
- During writing code, I devided everything on small steps, and later just composed it;
- Using of female header connertors, that allowed to use sensors, actuartors and MC in other project and change configuration.

**What desicions was bad:**
- It was working only as stand;
- Big amount of cheap female header connectors didn't give a stable connection  (falling appart);
- Splitted PCB on 2 parts (falling appart); 
- Bad and weak power supplies (it could restart because of "huge" load);
- Code was such a mess, I didn't use funcions enough;
- Excessive components (some of them was unnecessary, like IR-remote).

**How I would do it now:**
- Change Arduino to ESP8266 (smth that have WI-FI module). And I would have access through telegram bot;
- Don't try to add everything you can in hardware/software (IR control, SD-card, camera, energy saving, battery mode);
- Choose functionality as small as you can (I mean don't add battery, if supposed to work from grid);
- Control panel change to encoder;
- Use EEPROM to save main parameters;
- Don't make compositions of 2 PCB, voltage regulator. Make it all on one board;
- Use good header connectors;
- Devide code on few files (main, functions, pics);
- The more simple code the better;
- Dont' be shy to use resistors;
- Comment your code;
- Think about power supply and its power;

Anyway it was my first experience in making smth like this and developing PCB's. Good that I took lessons out of this.

