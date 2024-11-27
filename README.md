# aws-esp32 ##Saran Kongthong


# IoT Data Collection and Visualization

This project involves collecting data from a DHT11 sensor using an ESP32, sending the data through MQTT to AWS IoT Core, and then storing the data in Amazon DynamoDB and an S3 bucket. The data is also visualized using AWS Amplify. The system listens to specific MQTT topics and processes incoming data.

## Table of Contents

- [Description](#description)
- [Features](#features)
- [Architecture](#architecture)
- [Requirements](#requirements)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)






![image](https://github.com/user-attachments/assets/c230aaad-1404-407a-993d-5d84c6213af1)



## Description

This project connects an ESP32 to a DHT11 sensor to collect temperature and humidity data. The data is then transmitted to AWS IoT Core over MQTT, where it is routed and stored in two places:
1. **DynamoDB** – Data is stored in a NoSQL database for further processing and retrieval.
2. **S3 Bucket** – Data is stored in an S3 bucket for backup or storage purposes.
   
Additionally, the project uses AWS Lambda functions to process and transform incoming data as needed. AWS Amplify is used to visualize the data in a user-friendly interface.

## Features

- Collects temperature and humidity data using the DHT11 sensor and ESP32.
- Sends data to AWS IoT Core via MQTT.
- AWS IoT Core routes data to DynamoDB and S3 Bucket.
- DynamoDB and S3 Bucket listen to `+/telemetry` topics for incoming data.
- Presence events (online/offline) are routed based on the topic `$aws/events/presence/+/+`.
- Data is visualized and monitored through AWS Amplify.

## Architecture

![Architecture Diagram](./image.png)

### Key Components:
1. **DHT11 Sensor**: Reads temperature and humidity data.
2. **ESP32**: Acts as the microcontroller to collect sensor data and send it to AWS IoT Core using MQTT.
3. **AWS IoT Core**: The central hub for receiving and routing MQTT messages.
4. **Amazon DynamoDB**: Stores telemetry data.
5. **Amazon S3 Bucket**: Stores telemetry data for backup.
6. **AWS Lambda**: Processes incoming data for further operations.
7. **AWS Amplify**: Visualizes the collected data in a web interface.

## Requirements

Before starting, ensure you have the following:

- **Hardware**:
  - ESP32 development board.
  - DHT11 sensor.
- **Software**:
  - AWS account with DynamoDB, S3, and IoT Core configured.
  - Platform for flashing the ESP32, e.g., Arduino IDE or PlatformIO.
  - Node.js and AWS Amplify installed for visualizing data.

## Setup Instructions

### Step 1: Setup AWS IoT Core

1. **Create an AWS IoT Thing**:
   - Navigate to the AWS IoT Core console and create a new "Thing" to represent the ESP32.
   - Download the IoT certificates for authentication (public, private key, and certificate).

2. **Configure MQTT Topics**:
   - Create a message routing rule for `+/telemetry` to direct data to DynamoDB and S3 Bucket.
   - Create a rule for `$aws/events/presence/+/+` to monitor device presence.

3. **Create DynamoDB Table**:
   - Create a DynamoDB table to store telemetry data. You can define a simple schema with "deviceId" as the primary key.

4. **Create S3 Bucket**:
   - Create an S3 bucket for storing telemetry data. Ensure the correct permissions are set for AWS IoT Core to write data to this bucket.

5. **Setup AWS Lambda**:
   - Create a Lambda function to process the incoming data from MQTT.
   - Connect the Lambda function to the `+/telemetry` and `$aws/events/presence/+/+` rules.

### Step 2: Setup the ESP32 and DHT11

1. **Install Libraries**:
   - Install the required libraries for the DHT11 sensor, MQTT, and ESP32.
   - Example libraries:
     - `Adafruit DHT` for reading from the DHT11 sensor.
     - `PubSubClient` for MQTT communication.

2. **Flash the ESP32**:
   - Use Arduino IDE or PlatformIO to upload the firmware to your ESP32.
   - Configure the ESP32 to connect to your WiFi and publish data to AWS IoT Core over MQTT.

### Step 3: AWS Amplify Setup

1. **Create Amplify Project**:
   - Set up an AWS Amplify project to visualize the telemetry data.
   - Connect Amplify to your backend services (DynamoDB or S3) to display data.

2. **Connect Amplify to DynamoDB or S3**:
   - Use the AWS Amplify CLI to configure data fetching from DynamoDB or S3, depending on your visualization needs.

### Step 4: Test the Setup

1. **Power on ESP32** and monitor data being sent to AWS IoT Core.
2. **Check DynamoDB** and **S3 Bucket** for incoming data.
3. **View the data** in the AWS Amplify interface.

## Usage

Once the setup is complete, the ESP32 will continuously send temperature and humidity data from the DHT11 sensor to AWS IoT Core. The data will be routed to DynamoDB and an S3 bucket for storage. You can use AWS Amplify to visualize the data.

1. **Monitor device presence** via AWS IoT Core and check status with the `$aws/events/presence/+/+` rule.
2. **View telemetry data** through the AWS Amplify web interface.

## Contributing

If you would like to contribute to this project, feel free to fork the repository and create pull requests. Please ensure that your contributions adhere to the following:

1. Write clear and concise commit messages.
2. Document any changes in the README.
3. Test your changes locally before pushing.

## License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

