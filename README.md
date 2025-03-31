# STM32 Micro-ROS Integration

## Overview
This project integrates an STM32 microcontroller with ROS 2 using Micro-ROS. The firmware facilitates communication between the microcontroller and ROS 2 via serial communication. It provides various functionalities such as motor control, sensor data acquisition, and relay control.

## Features
- Micro-ROS integration with STM32
- Sensor data publishing
- Motor and relay control services
- Serial communication via USB

## Requirements
### Hardware
- STM32 Blackpill (STM32F411CEU6)
- USB cable for data transfer
- Jumper wires
- Required sensors and actuators

### Software Dependencies
Ensure you have the following installed:
- [PlatformIO](https://platformio.org/install)
- [Docker](https://docs.docker.com/get-docker/)
- ROS 2 (Jazzy, Humble or compatible version)
- Micro-ROS Agent Docker Image

## Installation
1. Clone the repository:
   ```sh
   git clone https://github.com/yourusername/stm_ros2.git
   cd stm_ros2
   ```
2. Install dependencies:
   ```sh
   pip install platformio
   . add_pio_cli.sh
   ```
3. Build the firmware:
   ```sh
   pio run
   ```

## Uploading the Firmware
### Using DFU Mode
1. Disconnect the USB and any other power source.
2. Connect `PA10` to `GND` using a jumper wire.
3. Connect the USB cable to the Blackpill board.
4. Hold the **BOOT0** button and then press the **NRST** button quickly.
5. Release the **BOOT0** button. The board should now be in DFU mode.
6. Upload the firmware:
   ```sh
   pio run --target upload
   ```

## Testing the System
1. Ensure all hardware connections are secure.
2. Run the Micro-ROS agent using Docker:
   ```sh
   docker run -it --rm --name mros -v /dev:/dev --privileged --net=host microros/micro-ros-agent:jazzy serial --dev /dev/ttyUSB0 -b 115200 -v6
   ```
3. Verify the firmware upload:
   ```sh
   pio run --target upload
   ```
   - If the upload fails, try troubleshooting:
     - Replug the ST-Link or USB connection.
     - Detach the STM32 from the PCB and retry.
     - If the ST-Link is faulty, modify `platformio.ini` to use `dfu` as the upload protocol instead of `stlink`.

4. Test hardware communication via ROS 2:
   - Publish data to a topic:
     ```sh
     ros2 topic pub /data_msg std_msgs/Int8 "{data: 0}"
     ```
   - Subscribe to sensor data:
     ```sh
     ros2 topic echo /sensor_topic
     ```

## Troubleshooting
- If the firmware fails to upload:
  - Ensure the STM32 is in DFU mode.
  - Check USB connections and try different cables.
  - Restart the system and try again.

- If ROS 2 communication fails:
  - Verify that the Micro-ROS agent is running.
  - Check the serial baud rate matches in the firmware and the agent.
  - Use `dmesg | grep ttyUSB` to check if the USB device is recognized.

## Contributors
- Nilanjan Chowdhury

