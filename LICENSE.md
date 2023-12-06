// Define the pin to which the sensor is connected
const int pulsePin = A0;

// Variables for heart rate calculation
int heartRateValue;
int heartRateAverage;
int samplesCounter;

void setup() {
  Serial.begin(9600); // Initialize serial communication

  heartRateValue = 0;
  heartRateAverage = 0;
  samplesCounter = 0;
}

void loop() {
  int sensorValue = analogRead(pulsePin); // Read analog value from the sensor
  heartRateValue += sensorValue; // Add the value to the heart rate sum
  samplesCounter++; // Increment the sample counter

  // Collect a certain number of samples before calculating the average
  if (samplesCounter == 10) {
    heartRateAverage = heartRateValue / samplesCounter; // Calculate the average
    int heartRate = map(heartRateAverage, 0, 1023, 0, 100); // Map the value to a heart rate range
    Serial.println("Heart Rate: " + String(heartRate) + " BPM");
    
    // Reset variables for the next set of samples
    heartRateValue = 0;
    heartRateAverage = 0;
    samplesCounter = 0;
  }

  delay(10); // Delay for stable readings
}
