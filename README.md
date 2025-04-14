# Rideau-Canal-Skateway
Real-time monitoring system for Rideau Canal Skateway using Azure IoT.

## <ins>1. Overview of the Rideau Canal Skateway Monitoring Scenario:</ins>
The Rideau Canal Skateway in Ottawa is the world’s largest naturally frozen skating rink and a popular winter destination for locals and tourists alike. Ensuring the safety of skaters is a top priority for the National Capital Commission (NCC), especially given the variable and sometimes unpredictable nature of ice and weather conditions. To maintain safety, there’s a need for a reliable and real-time monitoring system that can track environmental conditions and detect hazards before they pose risks to the public.

## Problem Addressed by the Solution:
The core problem is the lack of continuous, real-time monitoring of the ice and weather conditions across the length of the canal. Without this, there’s a risk of:
-	Skaters being exposed to thin or weak ice,
-	Delays in responding to deteriorating weather that affects ice safety,
-	Inefficient or delayed decision-making regarding closures or maintenance.

## Proposed Solution:
To address this, a real-time data streaming system is implemented with the following components:
1.	Simulated IoT Sensors: These mimic real devices placed along the canal to monitor critical factors such as:
o	Ice thickness
o	Surface temperature
o	Ambient temperature
o	Humidity
o	Wind speed and precipitation
2.	Real-Time Data Processing: As data streams in, it's analyzed instantly to identify:
o	Dangerous ice conditions (e.g., thin ice or rapidly warming temperatures)
o	Weather patterns that could lead to unsafe skating conditions
3.	Data Storage in Azure Blob Storage: All processed data is stored securely for:
o	Historical analysis
o	Reporting and visualization
o	Training predictive models in the future
Benefits:
•	Enhanced public safety through timely alerts and preventive action.
•	Data-driven decision-making for NCC officials.
•	Long-term insights into climate and usage patterns on the canal.
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
Device authentication (via unique keys or certificates)
Message encryption
Access control

✅ Setup Steps
1. 	Create a Resource Group.
2.	Create an IoT Hub via Azure Portal.
3.	Register a new IoT devices (real or simulated).
4.	Copy the connection string for device-to-cloud communication.
5.	Route incoming messages to Azure Stream Analytics.
![image](https://github.com/user-attachments/assets/4618a8e5-918e-4593-b066-92a033813ec1)
**_Primary connection string Example: _**
HostName=IOThubcst8916.azure-devices.net;DeviceId=sensor1;SharedAccessKey=MdetcJkxW4a/zE3O4GsqAOHKJZefitzJnEoiwv1agtU=

### 3.3 Azure Stream Analytics Job:

### 3.4 o	Azure Blob Storage:
