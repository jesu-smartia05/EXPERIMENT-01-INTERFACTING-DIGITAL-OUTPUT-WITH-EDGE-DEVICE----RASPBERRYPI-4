# EXPERIMENT-01-INTERFACTING-DIGITAL-OUTPUT-WITH-EDGE-DEVICE---(RASPBERRYPI-PI4)
### NAME :Jesu Smartia A
### DEPARTMENT : CSE(IoT)
### ROLL NO : 212223110016
### DATE OF EXPERIMENT :4.2.26

### AIM
To interface a digital output device (LED) with the Raspberry Pi 4 and control it using Python.

## APPARATUS REQUIRED
Raspberry Pi 4
LED (Light Emitting Diode)
330Ω Resistor
IR Sensor
Breadboard
Jumper Wires
USB Cable
 ## THEORY

![Raspberry Pi Pin](https://github.com/user-attachments/assets/19e5a1e7-cb46-4909-ba59-e4f4560cae03)





 
 
 
 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM 


The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.


## Working Principle:
Experiment 1A
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to GP15 via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).

Experiment 1B
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The IR sensor is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF based on the IR sensor.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to any one GPIO via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).
Connect the IR sensor Vcc to any +5V.
Connect the IR sensor GND to any GND.
Connect the IR sensor OUT to any one GPIO. 

## PROGRAM (Python)
```


 



 
````

### OUPUT  
# Experiment 1A
## PROGRAM:
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "MTJU2K2KAIGY5WFQ"
CHANNEL_ID = 3249739
THINGSPEAK_URL = "https://api.thingspeak.com/update"

# Set GPIO numbering mode
GPIO.setmode(GPIO.BCM)

# Define LED pin
LED_PIN = 18

# Set GPIO18 as output
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=MTJU2K2KAIGY5WFQ&field1={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)

try:
    while True:
        # LED ON
        GPIO.output(LED_PIN, GPIO.HIGH)
        print("LED ON")
        send_to_thingspeak(1)
        time.sleep(15)

        # LED OFF
        GPIO.output(LED_PIN, GPIO.LOW)
        print("LED OFF")
        send_to_thingspeak(0)
        time.sleep(15)

except KeyboardInterrupt:
    print("Program stopped")

finally:
    GPIO.cleanup()
```

# LED ON:
![WhatsApp Image 2026-02-05 at 1 16 51 PM](https://github.com/user-attachments/assets/3df57f6b-72c8-4ed5-8489-f34da0cb9808)

<img width="1494" height="826" alt="Screenshot 2026-02-04 112901" src="https://github.com/user-attachments/assets/945ea8c0-63fc-432b-b33d-19c49b88f3d6" />

![WhatsApp Image 2026-02-05 at 1 16 51 PM1](https://github.com/user-attachments/assets/27d5057a-49e8-4370-9e67-03dae0d5ea79)

# LED OFF:
![WhatsApp Image 2026-02-05 at 1 16 52 PM2](https://github.com/user-attachments/assets/1bfbb864-b9e4-4659-ab31-bfb2f590a459)

![WhatsApp Image 2026-02-05 at 1 16 50 PM](https://github.com/user-attachments/assets/162fab63-7e14-47a2-8ad8-7a73025faef0)

<img width="1632" height="822" alt="Screenshot 2026-02-04 112847" src="https://github.com/user-attachments/assets/9f4d9e59-0f9d-4168-b0a1-2c79786b6f9f" />

# Experiment 1B
## PROGRAM:
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "O8K2ODGZ0LFNFZ2W"
CHANNEL_ID = 3249686
THINGSPEAK_URL = "https://api.thingspeak.com/update"



# Pin setup
SENSOR_PIN = 23   # Input from sensor
LED_PIN = 18      # Output to LED

# GPIO mode
GPIO.setmode(GPIO.BCM)

# Setup pins
GPIO.setup(SENSOR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=O8K2ODGZ0LFNFZ2W&field1={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)


print("Sensor + LED system running...")

try:
    while True:
        sensor_value = GPIO.input(SENSOR_PIN)

        if sensor_value == 0:   # Many IR sensors give LOW when object detected
            print("Object Detected! LED ON")
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_to_thingspeak(1)

            time.sleep(15)
        else:
            print("No Object. LED OFF")
            GPIO.output(LED_PIN, GPIO.LOW)
            send_to_thingspeak(0)

            time.sleep(15)

        time.sleep(0.1)

except KeyboardInterrupt:
    print("Stopped by user")

finally:
    GPIO.cleanup()
```
# LED ON:
![WhatsApp Image 2026-02-05 at 2 35 17 PM](https://github.com/user-attachments/assets/eec6a960-3113-4107-b047-f8e9aa7ec145)

<img width="1600" height="830" alt="Screenshot 2026-02-05 142820" src="https://github.com/user-attachments/assets/7fa1826a-8ed3-432b-895f-2d958d79cf4b" />

![WhatsApp Image 2026-02-05 at 2 35 27 PM](https://github.com/user-attachments/assets/84f67e4b-e580-421e-bc67-b2e24bf6fc11)

# LED OFF:
![WhatsApp Image 2026-02-05 at 2 35 20 PM](https://github.com/user-attachments/assets/d55da7ed-8fc3-4f02-9fdb-0bdfde574d14)

<img width="1664" height="796" alt="Screenshot 2026-02-05 142808" src="https://github.com/user-attachments/assets/74b502c9-954a-42c5-9557-34f3e269bfa7" />

![WhatsApp Image 2026-02-05 at 2 35 28 PM](https://github.com/user-attachments/assets/5f7de105-5668-47f5-9651-861827c00c67)

# RESULT:
Thus, the program to interface digital output with edge device is executed successfully.
