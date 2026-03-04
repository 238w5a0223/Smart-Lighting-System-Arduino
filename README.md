# Interfacing a sensor:
Smart Temperature Automation System with LCD, Fan & Buzzer
Components Used: •	Arduino UNO
•	LM35 Temperature Sensor
•	16x2 LCD Display
•	Relay Module
•	DC Fan
•	Buzzer
•	Breadboard
•	Jumper Wires

![Circuit](C:\Users\kusum\OneDrive\Desktop\TASK - 1.png)
Code : 
#include <LiquidCrystal.h>
LiquidCrystal lcd(7,6,5,4,3,2);
int sensorPin = A0;
int relayPin = 9;
int buzzerPin = 8;
float threshold = 30.0;

void setup()
{
  lcd.begin(16,2);
  Serial.begin(9600);
  pinMode(relayPin,OUTPUT);
  pinMode(buzzerPin,OUTPUT);
}

void loop()
{
  int reading = analogRead(sensorPin);
  float voltage = reading * 5.0 / 1023.0;
  float temperature = voltage * 100.0;
  
  lcd.setCursor(0,0);
  lcd.print("Temp: ");
  lcd.print(temperature);
  lcd.print(" C ");
  
  Serial.print("Temperature: ");
  Serial.println(temperature);
  
  if(temperature > threshold)
  {
    digitalWrite(relayPin,HIGH);
    digitalWrite(buzzerPin,HIGH);
  }
  else
  {
    digitalWrite(relayPin,LOW);
    digitalWrite(buzzerPin,LOW);
  }
  
  delay(1000);
}
