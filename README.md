# Interfacing a sensor: (Temperature Sensor – TMP-36)
Temperature detection using Arduino uno & TMP 36 Sensor.
Components Used: List the Arduino, TMP36, resistor, and LED
![Circuit](C:\Users\kusum\OneDrive\Desktop\TASK - 1.png)
Code : 
const int sensorPin = A0; 
const int ledPin = 13;
const float threshold = 30.0; 
void setup() {
  Serial.begin(9600);       
  pinMode(ledPin, OUTPUT);  
}
void loop() {
   int sensorVal = analogRead(sensorPin);
   float voltage = (sensorVal / 1024.0) * 5.0;
 float temperature = (voltage - 0.5) * 100;
  
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" C");

  if (temperature > threshold) {
    digitalWrite(ledPin, HIGH);
    Serial.println("ALERT: High Temperature Detected!"); 
  } else {
    digitalWrite(ledPin, LOW);
  }
  delay(1000); 
}
