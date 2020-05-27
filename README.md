#include <LiquidCrystal.h>
LiquidCrystal LCD(11,10,9,2,3,4,5); //Arduino pin tied to pins of LCD display
int trigPin = 13;  // Arduino pin tied to trigger pin on the ultrasonic sensor
int echoPin = 12; // Arduino pin tied to echo pin on the ultrasonic sensor.
void setup() 
{ 
pinMode(1,OUTPUT);  //Arduino pin connected to relay module as output
pinMode(trigPin,OUTPUT); //Arduino pin connected to ultrasonic sensor as output 
pinMode(echoPin,INPUT);  //Arduino pin connected to ultrasonic sensor as input
pinMode(6,OUTPUT);//Arduino pin connected to buzzer as output
  LCD.begin(16,2); 
  LCD.setCursor(0,0);
  LCD.print("Target Distance:");
}
void loop()  {
 int temp;
 float duration,distance;
 digitalWrite(trigPin,LOW);  //output of trigpin
 delayMicroseconds(2); //make delay of 2 microseconds
 digitalWrite(trigPin,HIGH); //output of trigpin
 delayMicroseconds(10); //make delay of 10 microseconds
 digitalWrite(trigPin,LOW); //output of trigpin
 duration=pulseIn(echoPin,HIGH); //output of echopin
 distance = duration*340/20000; //formula of calculating distance

 if(distance<4) //condition for motor off
{
digitalWrite(1,LOW); //output of relay module
digitalWrite(6,HIGH); //output of BUZZER
LCD.clear();
LCD.print("Water Tank full"); //display in LCD screen
LCD.setCursor(0,1);
LCD.print("Motor turned OFF"); //display in LCD screen 
delay(500); //make delay of 500 microseconds
temp=1;
}
else if(distance<4&&temp==1) //condition for motor off
{
digitalWrite(1,LOW); //output of relay module
digitalWrite(6,HIGH); //output of buzzer
LCD.clear();
LCD.print("Water Tank full"); //display in LCD screen
LCD.setCursor(0,1);
LCD.print("Motor turned OFF"); //display in LCD screen
delay(500); //make delay of 500 microseconds
}
else  if(distance>23) //condition for motor on
{
digitalWrite(1,HIGH); //output of relay module
digitalWrite(6,LOW); //output of buzzer
LCD.clear();
LCD.print("Water Tank Empty"); //display in LCD screen
LCD.setCursor(0,1);
LCD.print("Motor turned ON"); //display in LCD screen
delay(500); //make delay of 500 microseconds
temp=0;
}
}
