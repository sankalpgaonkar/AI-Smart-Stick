# AI-Smart-Stick
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
#include <TMRpcm.h>

TMRpcm tmrpcm; //create a TMRpcm object

int left_trigPin = 7;
int left_echoPin = 6;
int right_trigPin = 2;
int right_echoPin = 3;
int center_trigPin = 4;
int center_echoPin = 5;

void setup() {
  pinMode(left_trigPin, OUTPUT);
  pinMode(left_echoPin, INPUT);
  pinMode(right_trigPin, OUTPUT);
  pinMode(right_echoPin, INPUT);
  pinMode(center_trigPin, OUTPUT);
  pinMode(center_echoPin, INPUT);
  Serial.begin(115200);
  
  tmrpcm.speakerPin = 9; //set the speaker pin
  tmrpcm.setVolume(5); //set the volume (0 to 7)
  
  if (!SD.begin(10)) { //initialize the SD card with the chip select pin
    Serial.println("SD card initialization failed");
    return;
  }
  
  if (!SD.exists("left.wav")) { //check if the file exists
    Serial.println("left.wav not found");
  }
  if (!SD.exists("right.wav")) {
    Serial.println("right.wav not found");
  }
  if (!SD.exists("ahead.wav")) {
    Serial.println("ahead.wav not found");
  }
}

void loop() {
  left();
  right();
  center();
}

void left() {
  delay(500);
  Serial.println("\n");
  int duration, distance;
  digitalWrite(left_trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(left_trigPin, LOW);
  duration = pulseIn(left_echoPin, HIGH);
  distance = (duration / 2) / 29.1;
  if (distance < 30) {
    Serial.print("Left Distance");
    Serial.print(distance);
    tmrpcm.play("left.wav");
  }
}

void right() {
  delay(500);
  Serial.println("\n");
  int duration, distance;
  digitalWrite(right_trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(right_trigPin, LOW);
  duration = pulseIn(right_echoPin, HIGH);
  distance = (duration / 2) / 29.1;
  if (distance < 30) {
    Serial.print("Right Distance");
    Serial.print(distance);
    tmrpcm.play("right.wav");
  }
}

void center() {
  delay(500);
  Serial.println("\n");
  int duration, distance;
  digitalWrite(center_trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(center_trigPin, LOW);
  duration = pulseIn(center_echoPin, HIGH);
  distance = (duration / 2) / 29.1;
  if (distance < 30) {
    Serial.print("Center Distance");
    Serial.print(distance);
    tmrpcm.play("ahead.wav");
  }
}
