/* Fill-in information from Blynk Device Info here */
#define BLYNK_TEMPLATE_ID "TMPL6RmAPvmwP"
#define BLYNK_AUTH_TOKEN "tq_lPJiWvlUe4CQLTCf7FT8hOSjGxOIL"

/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial

#include<ESP8266WiFi.h>
#include<BlynkSimpleEsp8266.h>
#include<OneWire.h>
#include<DallasTemperature.h>
#define ONE_WIRE_BUS D5

OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);


// Define the pulse sensor settings
const int pulsePin = A0; // the pulse sensor pin
const int ledPin = D4; // the LED pin
int pulseValue; // the pulse sensor value
int bpm; // the heart rate in beats per minute

// Your WiFi credentials.
// Set password to "" for open networks.
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[]="Hewigoegein";//isi dengan nama wifi
char pass[]="nomadenexe01";//isi dengan pass wifi;
BlynkTimer timer;

void kendali(){
float t, Signal;
t=sensors.getTempCByIndex(0); //suhu badan
Signal = 0;
if (t >38){
digitalWrite(D4,LOW);
digitalWrite(D7,LOW);
} else {
digitalWrite(D4,HIGH);
digitalWrite(D7,HIGH);
}
}

void sendsensor()
{
float t,Signal;
Serial.print("Membaca Suhu...");
sensors.requestTemperatures();
t = sensors.getTempCByIndex(0);
Serial.println("DONE");
Serial.print("Suhu yang terbaca: ");
Serial.println(t);

Blynk.virtualWrite(V6,t);
}

void setup(void)
 {
  // Start the serial communication
  Serial.begin(9600);
  sensors.begin();

  // Connect to WiFi
  WiFi.begin(ssid, pass);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");

  // Connect to Blynk
  Blynk.begin(auth, ssid, pass);
  while (!Blynk.connected()) {
    Serial.println("Connecting to Blynk...");
    delay(1000);
  }
  Serial.println("Connected to Blynk");

  // Set up the pulse sensor
  pinMode(pulsePin, INPUT);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);

pinMode(D7,OUTPUT);
pinMode(LED_BUILTIN,OUTPUT);
timer.setInterval(2000L, sendsensor);
timer.setInterval(10L, kendali);
////////////////////////////////////////////////////////////////
 }

void loop(void)
 {
  // Read the pulse sensor value
  pulseValue = analogRead(pulsePin);

  // Detect the pulse
  if (pulseValue > 600) {
    digitalWrite(ledPin, HIGH); // turn on the LED
    delay(100); // wait for a short time
    digitalWrite(ledPin, LOW); // turn off the LED
    bpm = 60000 / pulseValue; // calculate the heart rate in beats per minute
    Serial.print("Heart rate: ");
    Serial.print(bpm);
    Serial.println(" BPM");

    // Send the heart rate to Blynk
    Blynk.virtualWrite(V1, bpm);
    delay(200);

    // Print the heart rate on the serial monitor
    String message = "Heart rate: " + String(bpm) + " BPM";
    Serial.println(message);
  }

  
  // Run the Blynk loop
  Blynk.run();
  timer.run();
}
