# robotic-arm-with-LCD-display
#include <Servo.h>
#include <LiquidCrystal.h>

// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

byte smiley[8] = {
  0b00000,
  0b00000,
  0b01010,
  0b00000,
  0b00000,
  0b01110,
  0b10001,
  0b00000
};
byte smiley2[8] = {
  0b00000,
  0b00000,
  0b01010,
  0b00000,
  0b00000,
  0b10001,
  0b01110,
  0b00000
};

int threshold=50;
int sensorpin=A0;
int buzzer=7;
int val;
int led=6;
Servo servo1;
Servo servo2;
Servo servo3;
int angle = 10;
int trigPin = 8;
int echoPin = 9;
long duration;
int distance;
int safetyDistance;

void setup() {
   lcd.createChar(0, smiley);
     lcd.createChar(1, smiley2);
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
pinMode(echoPin, INPUT); // Sets the echoPin as an Input
  servo1.attach(A1);
  servo2.attach(A2);
  servo3.attach(A3);
  servo1.write(angle);
  servo2.write(angle);
  servo3.write(angle);
  pinMode(sensorpin,INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(led,OUTPUT);
  Serial.begin(9600);
   lcd.begin(16, 2);
  lcd.clear();
}
void loop() 
{ 

  long duration, distance;
  digitalWrite(trigPin, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(trigPin, HIGH);
//  delayMicroseconds(1000); - Removed this line
  delayMicroseconds(10); // Added this line
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;
  lcd.setCursor(0,0);
lcd.print("I am Created by");
lcd.setCursor(0,1);
lcd.print("Swadhin Kumar");
delay(4000);
lcd.clear();
lcd.setCursor(0,0);
lcd.print("& Ravi Shankar");
lcd.setCursor(0,1);
lcd.print("Jha");
delay(4000);
lcd.clear();
lcd.setCursor(0,0);
lcd.print("Who is Guided By");
lcd.setCursor(0,1);
lcd.print("Dr.Neelesh Gupta");
delay(3000);
lcd.clear();
  if (distance <30  && distance>0)
{
   digitalWrite(buzzer, HIGH);
  delay(500);
  lcd.print("hello there");
  delay(2000);
  lcd.clear();
lcd.setCursor(0,0);
lcd.print("My name is");
lcd.setCursor(0,1);
lcd.print("Truphiya");
delay(2000);

  for(angle =0; angle<=45; angle++)
  {
     servo1.write(angle);
     delay(15);
  }
  delay(500);
  lcd.clear();
  lcd.setCursor(0,0);
lcd.print("You look good");
lcd.setCursor(0,1);
lcd.print("Today");


        for(angle=0; angle<=90; angle++)
{
             servo2.write(angle);
             delay(15);
}
        delay(2000);
 val=analogRead(sensorpin);
 delay(2000);
 if(val>= threshold)
 {              // scan from 0 to 1800 degrees
   for(angle = 0; angle < 180; angle++)  
  {                                  
    servo3.write(angle);
    delay(5);
     digitalWrite(led ,HIGH);
  }  
  lcd.clear();
  lcd.setCursor(0,0);
lcd.print("Pleasure to");
lcd.setCursor(0,1);
lcd.print("Meet You  ");    
lcd.write(byte(1));       

             delay(3000);
  // now scan back from 180 to 0 degrees
  for(angle = 180; angle > 0; angle--)    
  {                                
    servo3.write(angle);
    delay(5);
  }
  digitalWrite(led, LOW);
 }
 else{
    servo3.write(0);
    digitalWrite(led, LOW);
    lcd.clear();
  lcd.setCursor(0,0);
lcd.print("Don't you want");
lcd.setCursor(0,1);
lcd.print("to be friends?");
delay(3000);
lcd.clear();
  lcd.setCursor(0,0);
lcd.print("It' ok then  ");
lcd.write(byte(0));
 }
  delay(3000);
  for(angle = 90; angle > 0; angle--)    
  {                                
    servo2.write(angle);
    delay(15);
  }
  delay(3000);
  lcd.clear();
  lcd.setCursor(0,0);
lcd.print("Have a Good");
lcd.setCursor(0,1);
lcd.print("Day");
delay(3000);
lcd.clear();
  for(angle = 45; angle > 0; angle--)    
  {                                
    servo1.write(angle);
    delay(15);
  }
}
else
{
  
   digitalWrite(buzzer, LOW);
  servo1.write(0);
  servo2.write(0);
  // servo3.write(0);
}

if (distance >= 200 || distance <= 0){
    Serial.println("Out of range");
  }
  else {
    Serial.print(distance);
    Serial.println(" cm");
  }
  delay(500);
}
