# Person-Tracking-Control-System
#include <Adafruit_MotorShield.h>

// Arduino Human Following Robot
// Created By DIY Builder
// You have to install the AFMotor and NewPing library Before Uploading the sketch
// You can find all the required libraries from the Arduino library manager.
// Contact me on Instagram for any queries (Insta Id: diy.builder)
// Modified 7 Mar 2022
// Version 1.1

// Include the library code:
#include <NewPing.h>
#include <Servo.h>
#include <AFMotor.h>

#define RIGHT A2              // Right IR sensor connected to analog pin A2 of Arduino Uno:
#define LEFT A3               // Left IR sensor connected to analog pin A3 of Arduino Uno:
#define TRIGGER_PIN A1        // Trigger pin connected to analog pin A1 of Arduino Uno:
#define ECHO_PIN A0           // Echo pin connected to analog pin A0 of Arduino Uno:
#define MAX_DISTANCE 200      // Maximum ping distance:

unsigned int distance = 0;    // Variable to store ultrasonic sensor distance:
unsigned int Right_Value = 0; // Variable to store Right IR sensor value:
unsigned int Left_Value = 0;  // Variable to store Left IR sensor value:

NewPing sonar(TRIGGER_PIN, ECHO_PIN, MAX_DISTANCE);  // NewPing setup of pins and maximum distance:

// Create motor objects
AF_DCMotor Motor1(1, MOTOR12_1KHZ);
AF_DCMotor Motor2(2, MOTOR12_1KHZ);
AF_DCMotor Motor3(3, MOTOR34_1KHZ);
AF_DCMotor Motor4(4, MOTOR34_1KHZ);

Servo myservo; // Create a servo object to control the servo:
int pos = 0;   // Variable to store the servo position:

void setup() {
  // The setup function runs only once when powering on or resetting the board:
  Serial.begin(9600); // Initialize serial communication at 9600 bits per second:
  myservo.attach(10); // Servo attached to pin 10 of Arduino UNO

  for (pos = 90; pos <= 180; pos += 1) {
    // Moves servo from 90 degrees to 180 degrees:
    myservo.write(pos);  // Tell servo to move according to the value of 'pos' variable:
    delay(15);           // Wait 15ms for the servo to reach the position:
  }

  for (pos = 180; pos >= 0; pos -= 1) {
    // Moves servo from 180 degrees to 0 degrees:
    myservo.write(pos);  // Tell servo to move according to the value of 'pos' variable:
    delay(15);           // Wait 15ms for the servo to reach the position:
  }

  for (pos = 0; pos <= 90; pos += 1) {
    // Moves servo from 0 degrees to 90 degrees:
    myservo.write(pos);  // Tell servo to move according to the value of 'pos' variable:
    delay(15);           // Wait 15ms for the servo to reach the position:
  }

  pinMode(RIGHT, INPUT); // Set analog pin RIGHT as an input:
  pinMode(LEFT, INPUT);  // Set analog pin LEFT as an input:
}

void loop() {
  delay(50); // Wait 50ms between pings:
  distance = sonar.ping_cm(); // Send ping, get distance in cm, and store it in the 'distance' variable:
  Serial.print("distance: ");
  Serial.println(distance); // Print the distance in the serial monitor:

  Right_Value = digitalRead(RIGHT); // Read the value from the Right IR sensor:
  Left_Value = digitalRead(LEFT);   // Read the value from the Left IR sensor:
  Serial.print("RIGHT: ");
  Serial.println(Right_Value); // Print the right IR sensor value in the serial monitor:
  Serial.print("LEFT: ");
  Serial.println(Left_Value); // Print the left IR sensor value in the serial monitor:

  if ((distance > 1) && (distance < 15)) {
    // Check if the ultrasonic sensor's value stays between 1 to 15.
    // If the condition is true, the following statements will execute:

    // Move Forward:
    Motor1.setSpeed(130); // Define motor1 speed:
    Motor1.run(FORWARD);  // Rotate motor1 clockwise:
    Motor2.setSpeed(130); // Define motor2 speed:
    Motor2.run(FORWARD);  // Rotate motor2 clockwise:
    Motor3.setSpeed(130); // Define motor3 speed:
    Motor3.run(FORWARD);  // Rotate motor3 clockwise:
    Motor4.setSpeed(130); // Define motor4 speed:
    Motor4.run(FORWARD);  // Rotate motor4 clockwise;
  } else if ((Right_Value == 0) && (Left_Value == 1)) {
    // If the condition is true, the following statements will execute:

    // Turn Left:
    Motor1.setSpeed(150); // Define motor1 speed:
    Motor1.run(FORWARD);  // Rotate motor1 clockwise:
    Motor2.setSpeed(150); // Define motor2 speed:
    Motor2.run(FORWARD);  // Rotate motor2 clockwise:
    Motor3.setSpeed(150); // Define motor3 speed:
    Motor3.run(BACKWARD); // Rotate motor3 anticlockwise:
    Motor4.setSpeed(150); // Define motor4 speed:
    Motor4.run(BACKWARD); // Rotate motor4 anticlockwise:
    delay(150);
  } else if ((Right_Value == 1) && (Left_Value == 0)) {
    // If the condition is true, the following statements will execute:

    // Turn Right:
    Motor1.setSpeed(150); // Define motor1 speed:
    Motor1.run(BACKWARD); // Rotate motor1 anticlockwise:
    Motor2.setSpeed(150); // Define motor2 speed:
    Motor2.run(BACKWARD); // Rotate motor2 anticlockwise:
    Motor3.setSpeed(150); // Define motor3 speed:
    Motor3.run(FORWARD);  // Rotate motor3 clockwise:
    Motor4.setSpeed(150); // Define motor4 speed:
    Motor4.run(FORWARD);  // Rotate motor4 clockwise:
    delay(150);
  } else if (distance > 15) {
    // If the condition is true, the following statements will execute:

    // Stop:
    Motor1.setSpeed(0);   // Define motor1 speed:
    Motor1.run(RELEASE);  // Stop motor1:
    Motor2.setSpeed(0);   // Define motor2 speed:
    Motor2.run(RELEASE);  // Stop motor2:
    Motor3.setSpeed(0);   // Define motor3 speed:
    Motor3.run(RELEASE);  // Stop motor3:
    Motor4.setSpeed(0);   // Define motor4 speed:
    Motor4.run(RELEASE);  // Stop motor4:
  }
}
