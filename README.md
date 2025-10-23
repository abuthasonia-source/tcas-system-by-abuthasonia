#define trigPin 9
#define echoPin 10
#define buzzer 7
#define redPin 3
#define greenPin 5
#define bluePin 6

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  long duration;
  float distance;// Use float for meter precision

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration * 0.034 / 2)/100.0;// Convert to meters

  // Clean numeric output for Processing
  Serial.println(distance,3);//Print distances in meter with 3 decimals

  if (distance > 1.5) {
    setColor(0, 255, 0);     // Green
    noTone(buzzer);
  } 
  else if (distance > 1.0 ) {
    setColor(255, 255, 0);   // Yellow
    noTone(buzzer);
  } 
  else if (distance > 0.5) {
    setColor(255, 165, 0);   // Orange
    tone(buzzer, 1000, 100); 
  } 
  else {
    setColor(255, 0, 0);     // Red
    tone(buzzer, 2000, 300); 
  }

  delay(500);
}

void setColor(int r, int g, int b) {
  analogWrite(redPin, 255 - r);
  analogWrite(greenPin, 255 - g);
  analogWrite(bluePin, 255 - b);
}
