const int tempPin = A0;
const int fanPin = 9;
int tempC;
int pwmValue;

void setup() {
  pinMode(fanPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int sensorValue = analogRead(tempPin);
  tempC = sensorValue * 0.488;

  if (tempC < 23) {
    pwmValue = 0;
  } else if (tempC >= 23 && tempC < 33) {
    pwmValue = 128;
  } else {
    pwmValue = 255;
  }

  analogWrite(fanPin, pwmValue);
  Serial.print("Temperature: ");
  Serial.print(tempC);
  Serial.print(" C | PWM: ");
  Serial.println(pwmValue);
  delay(500);
}
