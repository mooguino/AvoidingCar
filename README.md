# AvoidingCar
Voici le code :
// Adafruit Motor shield library
// copyright Adafruit Industries LLC, 2009
// this code is public domain, enjoy!
// avec quelques adaptations
#define LED 17
#define DEL 16
#define trigPin 19 //Trig Ultrasons (sortie) 
#define echoPin 18
#include <AFMotor.h>
//#include <Servo.h> 

// DC motor on M2
AF_DCMotor motor(2);
AF_DCMotor motorOne(1);
// DC hobby servo
//Servo servo1;
// Stepper motor on M3+M4 48 steps per revolution
//AF_Stepper stepper(48, 2);

void setup() {
  Serial.begin(9600);           // set up Serial library at 9600 bps
  Serial.println("Motor party!");
  pinMode(trigPin, OUTPUT); //Trig est une sortie 
  pinMode(echoPin, INPUT); //Echo est une entrée
  // turn on servo
 // servo1.attach(9);
   
  // turn on motor #2 and #1
  motor.setSpeed(255);
  motor.run(RELEASE);
  motorOne.setSpeed(255);
  motorOne.run(RELEASE);
  pinMode(LED, OUTPUT);
  pinMode(DEL, OUTPUT);
}
  
//int i;

// Test the DC motor, stepper and servo ALL AT ONCE!
void loop() {
  long duration, distance; 
  digitalWrite(trigPin, LOW); 
  delayMicroseconds(2); 
  digitalWrite(trigPin, HIGH); 
  delayMicroseconds(10); //Trig déclenché 10ms sur HIGH 
  digitalWrite(trigPin, LOW); // calcul de la distance : 
  duration = pulseIn(echoPin, HIGH); // Distance proportionnelle à la durée de sortie 
  distance = duration*340/(2*10000); //Vitesse du son théorique 
  if(distance >= 20) {
  motor.run(FORWARD);
  motorOne.run(FORWARD);
  digitalWrite(LED, HIGH);  // met la DEL on
  digitalWrite(DEL, LOW);  // met la DEL on
  delay(500);
  digitalWrite(LED, LOW);   //met la DEL off
  digitalWrite(DEL, HIGH);   //met la DEL off
  delay(500);
} else {
 
  digitalWrite(LED, LOW);  // met la DEL on
  digitalWrite(DEL, LOW);   //met la DEL off
  motor.run(RELEASE);
  motorOne.run(RELEASE);
  delay(100);
  motor.run(BACKWARD);
  motorOne.run(FORWARD);
  delay(1000);
}
