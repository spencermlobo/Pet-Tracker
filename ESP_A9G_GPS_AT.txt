#include <SoftwareSerial.h>

SoftwareSerial A9GSerial(2, 3);  // RX, TX

void setup() 
{
  Serial.begin(115200);
  A9GSerial.begin(115200);
  delay(1000);
  sendATcommand("AT", 1000);
  sendATcommand("AT+CGNSPWR=1", 1000);
}

void loop() 
{
  String gps_data = getGPSData();
  if (gps_data.length() > 0) 
  {
    sendLocationData(gps_data);
  }
}

void sendATcommand(String command, int timeout) {
  String response = "";
  A9GSerial.print(command + "\r\n");
  long int time = millis();
  while ( (time + timeout) > millis()) {
    while (A9GSerial.available()) {
      char c = A9GSerial.read();
      response += c;
    }
  }
  Serial.print(response);
}

String getGPSData() {
  sendATcommand("AT+CGNSINF", 1000);
  String response = "";
  long int time = millis();
  while ( (time + 1000) > millis()) {
    while (A9GSerial.available()) {
      char c = A9GSerial.read();
      response += c;
    }
  }
  Serial.print(response);
  if (response.indexOf("$GNRMC") >= 0) {
    return response.substring(response.indexOf("$GNRMC"), response.indexOf("$GNVTG"));
  } else {
    return "";
  }
}

void sendLocationData(String gps_data) {
  String lat = gps_data.substring(20, 29);
  String lng = gps_data.substring(32, 42);
  String date = gps_data.substring(50, 56);
  String time = gps_data.substring(7, 13);
  String data = "{\"lat\": " + lat + ", \"lon\": " + lng + ", \"date\": " + date + ", \"time\": " + time + "}";
  Serial.println("Sending location data: " + data);
  // Use HTTP POST to send data to Traccar server
  // Replace "http://<traccar_server_address>:<port>/data" with your Traccar server address and port
  // and replace "<device_id>" with the ID of the device you want to track
  String post_data = "id=867959034971787&data=" + data;
  String post_request = "POST /data HTTP/1.1\r\n";
  post_request += "Host: https://demo4.traccar.org:5055\r\n";
  post_request += "Content-Type: application/x-www-form-urlencoded\r\n";
  post_request += "Content-Length: " + String(post_data.length()) + "\r\n";
  post_request += "Connection: close\r\n\r\n";
  post_request += post_data;
  Serial.println(post_request);
  A9GSerial.print("AT+CIPSTART=\"TCP\",\"<traccar_server_address>\",<port>\r\n");
  delay(1000);
  A9GSerial.print("AT+CIPSEND=" + String(post_request.length()) + "\r\n");
  delay(1000);
  A9GSerial.print(post_request);
  delay(1000);
  A9GSerial.print("AT+CIPCLOSE\r\n");
}