/*
  SunAid - Smart Solar Monitoring and Control
  ESP32 + voltage sensor + relay + Blynk

  Before uploading: fill in your Blynk template, token and WiFi below.
  NEVER commit your real token or WiFi password to GitHub.
*/

#define BLYNK_PRINT Serial

#define BLYNK_TEMPLATE_ID   "YourTemplateID"
#define BLYNK_TEMPLATE_NAME "sunaid"
#define BLYNK_AUTH_TOKEN    "YourAuthToken"

#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

// WiFi credentials
char ssid[] = "YourWiFiName";
char pass[] = "YourWiFiPassword";

#define RELAY_PIN 23      // Relay input (IN) pin
#define SENSOR_PIN 35     // Voltage sensor signal (S) pin

float vin = 0.0;
int value = 1;            // V1 from the app: 0 = load allowed, 1 = forced off

// Switch from the Blynk app (V1)
BLYNK_WRITE(V1) {
  value = param.asInt();
  Serial.print("V1: ");
  Serial.println(value);
}

void setup() {
  Serial.begin(9600);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, 1);   // relay off at start
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

void loop() {
  float sensor = analogRead(SENSOR_PIN);
  Serial.print("raw = ");
  Serial.println(sensor);

  // Convert the raw reading (0-4095) to 0-20 V (voltage sensor range)
  vin = map(sensor, 0.0, 4095.0, 0.0, 20.0);
  Serial.print("vin = ");
  Serial.println(vin);

  Blynk.virtualWrite(V0, vin);  // send voltage to the app

  // Relay is active-low: 0 = ON, 1 = OFF
  if (vin >= 2 && value == 0) {
    digitalWrite(RELAY_PIN, 0);
  } else {
    digitalWrite(RELAY_PIN, 1);
  }

  Blynk.run();
  delay(500);
}
