# Rideau-Canal-Skateway
Real-time monitoring system for Rideau Canal Skateway using Azure IoT.

## <ins>1. Overview of the Rideau Canal Skateway Monitoring Scenario</ins>
The Rideau Canal Skateway in Ottawa is the world’s largest naturally frozen skating rink and a popular winter destination for locals and tourists alike. Ensuring the safety of skaters is a top priority for the National Capital Commission (NCC), especially given the variable and sometimes unpredictable nature of ice and weather conditions. To maintain safety, there’s a need for a reliable and real-time monitoring system that can track environmental conditions and detect hazards before they pose risks to the public.

## Problem Addressed by the Solution:
The core problem is the lack of continuous, real-time monitoring of the ice and weather conditions across the length of the canal. Without this, there’s a risk of:
•	Skaters being exposed to thin or weak ice,
•	Delays in responding to deteriorating weather that affects ice safety,
•	Inefficient or delayed decision-making regarding closures or maintenance.

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
Let me know if you’d like a sample architecture diagram or code for simulating the IoT sensors!

# --------------------------------------------------------------------------


