# Pet Tracker (Smart Collar) using Ai Thinker A9G

![image](https://github.com/user-attachments/assets/d929414e-e827-420c-8553-148a8403f223)

-----

### Summary
The system employs the AI Thinker A9G development board to track pets' real-time location using GPS and GSM. The data is processed and visualized using the Arduino IDE and Traccar server.

![image](https://github.com/user-attachments/assets/f094792e-d689-4e68-9bec-fa1f752cb3d2)

### Components Used
AI Thinker A9G Development Board: Chosen for its compact size and low power consumption, this board integrates GPS and GSM capabilities necessary for tracking and data transmission.

### System
The system aims to ensure the safety of pets by providing real-time location tracking via GPS. The design includes the connectivity with the Ai THinker A9G board to enhance flexibility and ease of integration, this can be seen in the image below.

![image](https://github.com/user-attachments/assets/d282a2e1-c4a7-4365-8be1-156b14395a52) 

### Software Details
**Arduino IDE**: Used to program the ESP32 board and run AT commands on the AI Thinker A9G​. [click here](https://www.arduino.cc/en/software)
**Traccar Server & Dashboard**: Utilized for real-time visualization of GPS data on maps. It provides a user-friendly interface to monitor the pet's location​​. [click here](https://www.traccar.org/server/).
**Coolwatcher**: follow [link](https://ai-thinker-open.github.io/GPRS_C_SDK_DOC/en/c-sdk/burn-debug.html) to install and run commands on the board. Download **coolwatcher.exe** from the cooltools SDK.

You can also follow basic AT commands to check if the board is functioning

Here's the markdown table for the AT commands:

| Command     | Description               |
|:------------|:--------------------------|
| AT          | Test the AT startup       |
| AT+CMGF     | Use to set the SMS mode. Either text or PDU mode can be selected by assigning 1 or 0 in the command. |
| AT+CMGW     | Use to store message in the SIM. |
| AT+GPS=1    | Use to enable GPS. When this command is sent the GPS is turned On and the LED on module for GPS starts blinking.|
| AT+GPS=0    | Use to turn OFF GPS. After sending this command GPS is turned OFF and LED also stops blinking.|
| AT+GPSRD=1  | Use to read GPS data and display it. Data is in NMEA format.. |
| AT+GPSRD=0  | Use to stop reading the GPS data.|
| AT+LOCATION=1 | Use to get location data in the form of latitude and longitude.|
| AT+LOCATION=2 | Gives location in latitude and longitude format.|
