# Arduino-Smoke-Detector
A small smoke detector based on arduino.
Made by:-
1) Soham Biswas RA2511003011541
2) Rishi Verma RA2511003011569
3) Nikita Deb RA2511003011567
4) Niharika Das RA2511003011547
5) Jahnavi RA2511003011571

// Pin Definitions
#define MQ2PIN A0         // MQ-2 analog output to Analog Pin A0
#define BUZZER 8          // Buzzer connected to Digital Pin 8
#define LED 9             // LED connected to Digital Pin 9

// Smoke detection threshold (adjust based on your sensor calibration)
#define SMOKE_THRESHOLD 300  // Analog value threshold for smoke detection

// Variables
int mq2Value = 0;
bool smokeDetected = false;

void setup() {
  // Initialize Serial Monitor
  Serial.begin(9600);
  
  // Set pin modes
  pinMode(BUZZER, OUTPUT);
  pinMode(LED, OUTPUT);
  pinMode(MQ2PIN, INPUT);
  
  // Ensure buzzer and LED are off initially
  digitalWrite(BUZZER, LOW);
  digitalWrite(LED, LOW);
  
  // Warm-up message
  Serial.println("System Initializing...");
  Serial.println("MQ-2 sensor warming up (30 seconds recommended)...");
  delay(2000);
  Serial.println("System Ready!");
  Serial.println();
}

void loop() {
  // Read MQ-2 sensor
  mq2Value = analogRead(MQ2PIN);
  
  // Check if smoke is detected
  smokeDetected = (mq2Value > SMOKE_THRESHOLD);
  
  // Print table header
  Serial.println("╔════════════════════════════════════════════════════════════╗");
  Serial.println("║              SMOKE DETECTION SYSTEM                        ║");
  Serial.println("╠════════════════════════════════════════════════════════════╣");
  
  // MQ-2 sensor value row
  Serial.print("║ MQ-2 Sensor Value:  ");
  Serial.print(mq2Value);
  printSpaces(37 - String(mq2Value).length());
  Serial.println("║");
  
  // Smoke detection status row
  Serial.print("║ Smoke Detected:     ");
  if (smokeDetected) {
    Serial.print("YES - ALARM ACTIVE!");
    printSpaces(21);
  } else {
    Serial.print("NO");
    printSpaces(37);
  }
  Serial.println("║");
  
  Serial.println("╚════════════════════════════════════════════════════════════╝");
  Serial.println();
  
  // Activate alarm if smoke detected
  if (smokeDetected) {
    activateAlarm();
  } else {
    // Ensure buzzer and LED are off when no smoke
    digitalWrite(BUZZER, LOW);
    digitalWrite(LED, LOW);
  }
  
  // Delay before next reading
  delay(2000);
}

// Function to activate buzzer and LED for 5 seconds
void activateAlarm() {
  unsigned long startTime = millis();
  
  while (millis() - startTime < 5000) {  // Run for 5 seconds
    digitalWrite(BUZZER, HIGH);
    digitalWrite(LED, HIGH);
    delay(200);  // Beep pattern: ON for 200ms
    
    digitalWrite(BUZZER, LOW);
    digitalWrite(LED, LOW);
    delay(200);  // Beep pattern: OFF for 200ms
  }
  
  // Ensure they're off after alarm
  digitalWrite(BUZZER, LOW);
  digitalWrite(LED, LOW);
}

// Helper function to print spaces for table alignment
void printSpaces(int count) {
  for (int i = 0; i < count; i++) {
    Serial.print(" ");
  }
}
