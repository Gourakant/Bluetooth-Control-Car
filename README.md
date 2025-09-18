# Bluetooth-Control-Car
# Testing Code 01
#include <SoftwareSerial.h>

// ==== Motor Driver Pins ====
#define IN1 4
#define IN2 5
#define IN3 6
#define IN4 7
#define EN1 10   // Motor A Enable (PWM)
#define EN2 11   // Motor B Enable (PWM)

// ==== Bluetooth Pins ====
#define BT_RX 12  // Arduino RX ← BT TX
#define BT_TX 13  // Arduino TX → BT RX

SoftwareSerial bt(BT_RX, BT_TX); 
char data;

int motorSpeed = 200; // default speed (0-255)

void setup() { 
  Serial.begin(9600);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(EN1, OUTPUT);
  pinMode(EN2, OUTPUT);

  stoprobot();  // start with stopped motors

  bt.begin(9600);
}

void loop() {
  if (bt.available()) { 
    data = bt.read();
    Serial.println(data);

    switch (data) {
      case 'F': forward(); break;
      case 'B': reverse(); break;
      case 'L': left(); break;
      case 'R': right(); break;
      case 'S': stoprobot(); break;
      case '1': setSpeed(100); break;  // slow
      case '2': setSpeed(150); break;  // medium
      case '3': setSpeed(200); break;  // fast
      case '4': setSpeed(255); break;  // max
    }
  }
}

// ==== Movement Functions ====
void forward() {
  analogWrite(EN1, motorSpeed);
  analogWrite(EN2, motorSpeed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void reverse() {
  analogWrite(EN1, motorSpeed);
  analogWrite(EN2, motorSpeed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void left() {
  analogWrite(EN1, motorSpeed);
  analogWrite(EN2, motorSpeed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void right() {
  analogWrite(EN1, motorSpeed);
  analogWrite(EN2, motorSpeed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void stoprobot() {
  analogWrite(EN1, 0);
  analogWrite(EN2, 0);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void setSpeed(int spd) {
  motorSpeed = spd;
  Serial.print("Speed set to: ");
  Serial.println(motorSpeed);
}
