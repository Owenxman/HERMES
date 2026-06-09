<img width="6250" height="4181" alt="HERMES" src="https://github.com/user-attachments/assets/3fdb04f8-ad44-48a1-8808-55e5fc5da460" />

## Description

HERMES is my personal Swiss Army Board with a Gyro, Magnometer (GPS), Accelerometer, Pressure Sensor, Air Quality Sensor, Humidity Sensor and Temperature Sensor. Built as a YSWS project for my Hack Club RISE ILE. I designed it with the intent to use it as an IMU for a robotic dog but I turned it into a separate project after I realized that the dog would take longer than the timeframe available.

## Why I Chose These Components

### USB-C Connector

I chose USB-C because it's the current standard and makes the board easier to use with modern devices. It provides both power and data over a single connector, and the required CC resistors make it easy for the board to be recognized as a USB device.

### AP2112K 3.3V Regulator

The AP2112K was chosen to provide a stable 3.3V supply for the microcontroller and sensors. It only requires a few external components, is easy to implement, and can supply more than enough current for the application.

### FT231X USB-to-UART Converter

I used the FT231X to handle USB communication and programming. It is a reliable and well-supported chip that makes it easy to connect the board to a computer for firmware uploads and serial communication.

### ATmega328PB

The ATmega328PB was selected because it is a well-documented microcontroller with plenty of available resources and development tools. It provides all of the peripherals needed for this project while keeping the design simple and easy to work with.

### 8 MHz Crystal

An external crystal was used to provide a more accurate clock source than the internal oscillator. This helps improve timing accuracy and ensures reliable communication with other devices.

### BME680 Environmental Sensor

I chose the BME680 because it combines temperature, humidity, pressure, and gas sensing into a single package. Using one sensor instead of several separate sensors reduces board space and simplifies the overall design.

### ICM-20948 IMU

The ICM-20948 was selected because it combines a 3-axis accelerometer, gyroscope, and magnetometer into a single device. This allows the board to measure movement and orientation while keeping the component count low.

### ISP Programming Header

An ISP header was included so the microcontroller can be programmed directly if needed. This provides a backup programming method and makes debugging easier during development.

### LEDs

Status LEDs were added to provide quick visual feedback. The power LED confirms the board is powered correctly, while the other LED can be used for debugging and status indication.

### Decoupling Capacitors

Decoupling capacitors were placed near each IC to help filter noise on the power rail and improve stability. These are standard design practice and help ensure reliable operation.

### Reset Circuit

The reset circuit allows the microcontroller to be reset manually and also supports automatic reset during programming. This makes uploading firmware more convenient during development.

<img width="912" height="868" alt="image" src="https://github.com/user-attachments/assets/36e7a086-b9e8-4514-adac-807026a5774c" />

## PCB

I routed the PCB with the intent to try and make the board as compact as possible. I got the board down to an overall footprint of around 40mm x 40mm. This became a major challenge with the amount of different decoupling capacitors and trying to fit everything where it was needed. 

I added 3 mounting holes for M2 screws for easy integration into any case or project in the future.

<img width="1337" height="712" alt="image" src="https://github.com/user-attachments/assets/8f6b3abd-53a8-4581-a180-75c006882f99" />

## Frontend


