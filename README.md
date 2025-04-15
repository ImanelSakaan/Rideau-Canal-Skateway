# Rideau-Canal-Skateway
Real-time monitoring system for Rideau Canal Skateway using Azure IoT.

## <ins>1. Overview of the Rideau Canal Skateway Monitoring Scenario:</ins>
The Rideau Canal Skateway in Ottawa is the world’s largest naturally frozen skating rink and a popular winter destination for locals and tourists alike. Ensuring the safety of skaters is a top priority for the National Capital Commission (NCC), especially given the variable and sometimes unpredictable nature of ice and weather conditions. To maintain safety, there’s a need for a reliable and real-time monitoring system that can track environmental conditions and detect hazards before they pose risks to the public.

## Problem Addressed by the Solution:
The core problem is the lack of continuous, real-time monitoring of the ice and weather conditions across the length of the canal. Without this, there’s a risk of:
-	Skaters being exposed to thin or weak ice,
-	Delays in responding to deteriorating weather that affects ice safety,
-	Inefficient or delayed decision-making regarding closures or maintenance.
## Proposed Solution
To address this, a real-time data streaming system is implemented with the following components:
1. **Simulated IoT Sensors**  
   These mimic real devices placed along the canal to monitor critical factors such as:  
   - Ice thickness  
   - Surface temperature  
   - Ambient temperature  
   - Humidity  
   - Wind speed and precipitation
2. **Real-Time Data Processing**  
   As data streams in, it's analyzed instantly to identify:  
   - Dangerous ice conditions (e.g., thin ice or rapidly warming temperatures)  
   - Weather patterns that could lead to unsafe skating conditions
3. **Data Storage in Azure Blob Storage**  
   All processed data is stored securely for:  
   - Historical analysis  
   - Reporting and visualization  
   - Training predictive models in the future
### Benefits
- Enhanced public safety through timely alerts and preventive action  
- Data-driven decision-making for NCC officials  
- Long-term insights into climate and usage patterns on the canal

# --------------------------------------------------------------------------
## <ins>2. System Architecture:</ins>
<!--<div align="center">
  <img src="https://github.com/user-attachments/assets/b45c0972-9577-4a81-914e-2cd3c1078917" alt="image" />
</div> -->
<!-- This is a private note to myself. It won't appear on the GitHub page. -->
      
   ![Architecture Diagram](https://github.com/user-attachments/assets/221c66a2-cad0-4d37-a9ca-83bb2e722460)
# --------------------------------------------------------------------------
## <ins>3. Implementation for the Rideau Canal Skateway Monitoring System using Azure services:</ins>
### 3.1 IoT Sensor Simulation:
Simulated IoT sensors mimic real-time data generation related to ice conditions and weather factors, sending telemetry data to Azure IoT Hub using a Python script.
Example JSON payload:
```python
{
  "location": "Dow's Lake",
  "iceThickness": 27,
  "surfaceTemperature": -1,
  "snowAccumulation": 8,
  "externalTemperature": -4,
  "timestamp": "2024-11-23T12:00:00Z"
}
```
### Data Generation
The simulated sensor randomly generates values for:
- **location:** A fixed or selectable location (e.g., "Dow's Lake")
- **iceThickness:** Ice thickness in centimeters
- **surfaceTemperature:** Temperature at the surface of the ice (°C)
- **snowAccumulation:** Depth of snow on top of the ice (cm)
- **externalTemperature:** Air temperature above the ice (°C)
- **timestamp:** ISO 8601 format datetime

Simulation Script Snippet (Python)
```python
import time
import random
from azure.iot.device import IoTHubDeviceClient, Message

CONNECTION_STRING = "Your IoT Hub device connection string here"

def get_telemetry():
    return {
        "temperature": random.uniform(20.0, 40.0),
        "humidity": random.uniform(30.0, 70.0)
    }

def main():
    client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)

    print("Sending telemetry to IoT Hub...")
    try:
        while True:
            telemetry = get_telemetry()
            message = Message(str(telemetry))
            client.send_message(message)
            print(f"Sent message: {message}")
            time.sleep(10)
    except KeyboardInterrupt:
        print("Stopped sending messages.")
    finally:
        client.disconnect()

if __name__ == "__main__":
    main()
```
### 3.2 Azure IoT Hub Configuration:
Azure IoT Hub allows secure, bi-directional communication between your IoT devices and the cloud. It supports:
1. Device authentication (via unique keys or certificates)
2. Message encryption
3. Access control

✅ Setup Steps
1. 	Create a Resource Group.
2.	Create an IoT Hub via Azure Portal.
3.	Register a new IoT devices (real or simulated).
4.	Copy the connection string for device-to-cloud communication.
5.	Route incoming messages to Azure Stream Analytics.
![image](https://github.com/user-attachments/assets/4618a8e5-918e-4593-b066-92a033813ec1)

**Primary connection string example:**  
`HostName=IOThubcst8916.azure-devices.net;DeviceId=sensor1;SharedAccessKey=MdetcJkxW4a/zE3O4GsqAOHKJZefitzJnEoiwv1agtU=`

### Endpoints
- **Input:** Device-to-cloud messages  
- **Output:** Stream Analytics or Azure Blob Storage (optional fallback)

### 3.3 Azure Stream Analytics Job
Azure Stream Analytics (ASA) is a real-time data processing engine. It allows you to:
1. Ingest streaming data (e.g., IoT sensor data)  
2. Filter, transform, and aggregate the data  
3. Route the results to various outputs (dashboards, databases, alerts, etc.)

#### **Input Source**
- **Source type:** IoT Hub  
- **Format:** JSON

#### **Query Logic**
- Filters unsafe conditions (e.g., ice too thin or temperature too warm)  
- Aggregates or enriches data if needed

#### **Sample Query**
```python
   SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG(temperature) AS AvgTemperature,
    AVG(humidity) AS AvgHumidity,
    System.Timestamp AS EventTime
INTO
    [output]
FROM
    [input]
GROUP BY
    IoTHub.ConnectionDeviceId, TumblingWindow(second, 60)
```
![image](https://github.com/user-attachments/assets/aa66a60e-8319-49e1-9e72-e29b33703f9a)

**Output Destinations**
•	Output: Azure Blob Storage
•	Optional: Azure Table Storage or Power BI for dashboards

### 3.4 Azure Blob Storage
Data is typically stored in containers based on:
- `year/month/day/hour/`  
- **Example path:** `container/2025/04/13/10/sensor-001.json`
#### **File Naming Convention**
- Named by sensor and timestamp  
- **Example:** `sensor-001_20250413T103000Z.json`
#### **File Format**
- Default format: JSON
<img width="592" alt="image" src="https://github.com/user-attachments/assets/055e6539-e107-4a50-b80a-d24f7492e9ab" />

# --------------------------------------------------------------------------
## <ins>4. Usage Instructions:</ins>
### Running the IoT Sensor Simulation
Prerequisites:
•	Python 3.13.3
•	Azure IoT Device SDK:
•	pip install azure-iot-device
•	Your IoT Hub device connection string

1. Clone the repository to your local machine:
   ```bash
   git clone [https://github.com/ImanelSakaan/Rideau-Canal-Skateway.git]
2.  Navigate to the simulation directory in Visual Code:
   Rideau-Canal-Skateway/IOTSimulation
3.  Install the Azure IoT Device SDK:
![image](https://github.com/user-attachments/assets/d9032cb3-530b-4516-87b8-72398fda3e45)

4.  Run the following command in Visual Studio Code terminal:
  python -m pip install azure-iot-device
![image](https://github.com/user-attachments/assets/4457fcf5-d15a-43ee-81e3-5c92de51f3e9)

5. Replace connection string: In the script:
   connection_string = "<Your IoT Device Connection String>"
6.  Run sensor1.py:
   python sensor1.py
7.  Run sensor2.py:
   python sensor2.py
  ![image](https://github.com/user-attachments/assets/73574e9d-9015-4f0f-a217-4fcc57f61c5a)
8.	Check output: The script sends telemetry data every 10 seconds to Azure IoT Hub.

### Configuring Azure Services:
### A. Azure IoT Hub Setup
#### 1. Create IoT Hub
- Go to **Azure Portal** > **Create a Resource** > **IoT Hub**  
- Select a pricing tier (**Free** is sufficient for testing purposes)  
- Create the hub and wait for the deployment to complete
![image](https://github.com/user-attachments/assets/3cfc501e-bee0-4304-8028-9e3e5ce0be07)

#### 2. Add a Device
- Go to your **IoT Hub** > **Devices** > **Add Device**  
- Set a **Device ID** (e.g., `sensor-001`), then click **Create**  
- Copy the **Primary Connection String** — you'll need it in your Python script
<img width="846" alt="image" src="https://github.com/user-attachments/assets/8a0f52a1-6d58-4ba6-8593-523510319106" />

### B. Stream Analytics Job Setup
#### 1. Create a Stream Analytics Job
- Go to **Azure Portal** > **Create a Resource** > **Stream Analytics Job**  
- Give it a name, select the region, and choose a resource group
#### 2. Add Input
- In your job, go to **Inputs** > **Add Stream Input** > **IoT Hub**  
- Link your existing **IoT Hub** as the input source
#### 3. Add Output
- Go to **Outputs** > **Add** > **Blob Storage**  
- Select or create a **Blob Storage account** and **container**  
#### 4. Define Query
- Go to the **Query** tab  
- Paste the processing query (see example in previous section)
#### 5. Start the Job
- Once input, output, and query are configured, click **Start** > **Now**
![image](https://github.com/user-attachments/assets/dc1ae743-913f-48de-88ef-4a0c0a32d0da)

