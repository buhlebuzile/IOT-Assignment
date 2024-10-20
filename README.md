int noise = 2; // Connected to digital output of KY-038 sound sensor module
int yellowLed = 13; // Connected to positive of yellow LED
int redLed = 12; // Connected to positive of red LED
unsigned long yellowLedOnTime = 0; // Variable to store the last time the yellow LED was turned on
const unsigned long yellowLedDuration = 1000; // Duration (in milliseconds) to keep the yellow LED on after detecting sound

void setup() {
  Serial.begin(9600); // Start serial communication for debugging
  pinMode(noise, INPUT); // Setting the pin to input for reading data
  pinMode(yellowLed, OUTPUT); // Setting the pin to output for turning the yellow LED on/off
  pinMode(redLed, OUTPUT); // Setting the pin to output for turning the red LED on/off
}

void loop() {
  int data = digitalRead(noise); // Reading data from sensor and storing in variable
  Serial.println(data); // Print sensor data for debugging

  unsigned long currentTime = millis(); // Get the current time

  if (data == 1) { // 1 is sent from sensor when loud noise is detected
    // Turn the yellow LED on and update the timestamp
    digitalWrite(yellowLed, HIGH);
    yellowLedOnTime = currentTime;

    // Flash the red LED when sound is detected
    digitalWrite(redLed, HIGH);  // Turn the red LED on
    delay(200);                  // Wait for 200 milliseconds
    digitalWrite(redLed, LOW);   // Turn the red LED off
    delay(200);                  // Wait for 200 milliseconds
  } else {
    // Check if the yellow LED should still be on
    if (currentTime - yellowLedOnTime < yellowLedDuration) {
      digitalWrite(yellowLed, HIGH); // Keep the yellow LED on
    } else {
      digitalWrite(yellowLed, LOW); // Turn the yellow LED off
    }
    // Ensure the red LED is off when no sound is detected
    digitalWrite(redLed, LOW);
  }

  delay(50); // Small delay to allow the sensor to stabilize
}

