# Bluetooth-Control-Car
# Testing Code 01
#include <SoftwareSerial.h>

#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5
//#define EN1 6
//#define EN2 9
//

SoftwareSerial bt(11, 10); // RX, TX
char data;

void setup() { 
  Serial.begin(9600);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  //pinMode(EN1, OUTPUT);
  //pinMode(EN2, OUTPUT);

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
    }
  }
}

void forward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void reverse() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void left() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void right() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stoprobot() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
