# IoT Data Collection and Visualization

This project involves collecting data from a DHT11 sensor using an ESP32, sending the data through MQTT to AWS IoT Core, and then storing the data in Amazon DynamoDB and an S3 bucket. The data is also visualized using AWS Amplify. The system listens to specific MQTT topics and processes incoming data.

# Use Case for Temperature and Humidity Monitoring for Reporting to Tenant Association
Use Case Title: Temperature and Humidity Measurement for Optimal Indoor Environment and Reporting  
Primary Actor: Property Manager or Landlord

Goal: To continuously monitor and collect temperature and humidity data from rental properties during the winter months to ensure optimal living conditions and report to the tenant association if the temperature is too low.

## Table of Contents

- [Description](#description)
- [Features](#features)
- [Architecture](#architecture)
- [Requirements](#requirements)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
- [Contributing](#contributing)

## Architecture

![image](https://github.com/user-attachments/assets/89a659f6-e8fa-43dc-81ea-0121a7ff242d)

## DHT11 ESP32 Connection

![image](https://github.com/user-attachments/assets/10fe7c40-57bd-4a20-9fad-4b8345b25c42)

### Connection description
- ESP32 5v-VCC DHT11/DHT22
- ESP32 GPIO PIN 0-DATA DHT11/DHT22
- ESP32 GND-GND DHT11/DHT22

## Description

This project connects an ESP32 to a DHT11 sensor to collect temperature and humidity data. The data is then transmitted to AWS IoT Core over MQTT, where it is routed and stored in two places:
1. **DynamoDB** – Data is stored in a NoSQL database for further processing and retrieval.
2. **S3 Bucket** – Data is stored in an S3 bucket for backup or storage purposes.

Additionally, the project uses AWS Lambda functions to process and transform incoming data as needed. AWS Amplify is used to visualize the data in a user-friendly interface.


A notification is sent to a Discord channel when the temperature drops to 18°C or below.

To configure the Discord notification:
1. Create a Discord webhook in your server settings.
2. Store the webhook URL in AWS Secrets Manager or set it as an environment variable in AWS Lambda.
3. Ensure the Lambda function processes the temperature data and sends a notification if the temperature drops below the threshold.

## Features

- Collects temperature and humidity data using the DHT11 sensor and ESP32.
- Sends data to AWS IoT Core via MQTT.
- AWS IoT Core routes data to DynamoDB and S3 Bucket.
- DynamoDB and S3 Bucket listen to `+/telemetry` topics for incoming data.
- Presence events (online/offline) are routed based on the topic `$aws/events/presence/+/+`.
- Data is visualized and monitored through AWS Amplify.
- Sends a Discord notification when the temperature falls to 18°C or below.

### Key Components:
1. **DHT11 Sensor**: Reads temperature and humidity data.
2. **ESP32**: Acts as the microcontroller to collect sensor data and send it to AWS IoT Core using MQTT.
3. **AWS IoT Core**: The central hub for receiving and routing MQTT messages.
4. **Amazon DynamoDB**: Stores telemetry data.
5. **Amazon S3 Bucket**: Stores telemetry data for backup.
6. **AWS Lambda**: Processes incoming data for further operations.
7. **AWS Amplify**: Visualizes the collected data in a web interface.
8. **Discord Webhook**: Sends notifications to Discord when temperature drops below 18°C.

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
   - **Add Discord Notification Logic**: 
     - Securely store your Discord webhook URL in **AWS Secrets Manager** or as an **environment variable**.
     - Add the logic in the Lambda function to send a Discord notification when the temperature drops to 18°C or below.

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
4. **Verify Discord Notifications** by testing temperature drops to 18°C or below.

## Usage

Once the setup is complete, the ESP32 will continuously send temperature and humidity data from the DHT11 sensor to AWS IoT Core. The data will be routed to DynamoDB and an S3 bucket for storage. You can use AWS Amplify to visualize the data.

1. **Monitor device presence** via AWS IoT Core and check status with the `$aws/events/presence/+/+` rule.
2. **View telemetry data** through the AWS Amplify web interface.
3. **Receive Discord notifications** when the temperature falls to 18°C or below.


