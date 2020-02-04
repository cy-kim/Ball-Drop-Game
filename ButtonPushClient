//Code adapated from Tom Igoe's BallDropMKR1000JoystickClient.ino file

#include <SPI.h>
#include <WiFiNINA.h>
#include "arduino_secrets.h" 

char ssid[] = SECRET_SSID;        
char pass[] = SECRET_PASS;  
  
WiFiClient client;
int status = WL_IDLE_STATUS;
IPAddress server(10,23,10,149); 

//input pins
const int port = 8080;
const int buttonConnect = 12;
const int buttonLeft = 11;
const int buttonRight = 3;
const int buttonUp = 10;
const int buttonDown = 9;
const int signalLED = 2;

//button states
int buttonConnectState = HIGH;
int buttonLeftState = HIGH;
int buttonRightState = HIGH;
int buttonUpState = HIGH;
int buttonDownState = HIGH;

//intervals for reconnecting
const int sendInterval = 100;
const int debounceInterval = 15;

int prevButtonState = 0;
long lastTimeSent = 0;

void setup() {
  Serial.begin(9600);

  while (status != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    status = WiFi.begin(ssid, pass);
    delay(10000);
  }
  //printWiFiStatus();
  
  pinMode(buttonConnect, INPUT_PULLUP);    
  pinMode(buttonLeft, INPUT_PULLUP);   
  pinMode(buttonRight, INPUT_PULLUP); 
  pinMode(buttonUp, INPUT_PULLUP);   
  pinMode(buttonDown, INPUT_PULLUP);
  pinMode(signalLED, OUTPUT);
}

void loop() {
  char message = 0;
  long now = millis();
  boolean buttonPushed = buttonRead(buttonConnect);
  if (buttonPushed) {
      if (client.connected()) {
        Serial.println("disconnecting");
        client.print("x");
        client.stop();
      }
    else {
      Serial.println("connecting");
      client.connect(server, 8080);
    }
  }

  buttonLeftState = digitalRead(buttonLeft);
  buttonRightState = digitalRead(buttonRight);
  buttonUpState = digitalRead(buttonUp);
  buttonDownState = digitalRead(buttonDown);

  switch (buttonLeftState){
    case LOW:
      message = 'l';
      break;
  }

  switch (buttonRightState){
    case LOW:
      message = 'r';
      break;
  }
  switch (buttonUpState){
    case LOW:
      message = 'u';
      break;
  }
  
  switch (buttonDownState){
    case LOW:
      message = 'd';
      break;
  }
  //LED Connection
  if (client.connected() && now - lastTimeSent > sendInterval && message!=0){
    client.print(message);
    Serial.print(message);
    lastTimeSent = now;
  }

  digitalWrite(signalLED, client.connected());

  if (client.available()){
    char c = client.read();
    Serial.write(c);
  }
}

boolean buttonRead(int thisButton) {
  boolean result = false;
  int currentState = digitalRead (thisButton);
  if (currentState != prevButtonState && currentState == LOW) {
    result = true;
    }
  delay(debounceInterval);
  prevButtonState = currentState;
  return result;
    }
 
void printWifiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your board's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}
