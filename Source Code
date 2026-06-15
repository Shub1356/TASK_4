#include <WiFi.h>
#include <WiFiClient.h>


#define BLYNK_TEMPLATE_ID "TMPL3UHa5excW"
#define BLYNK_TEMPLATE_NAME "smart lightening"
#define BLYNK_AUTH_TOKEN "NM-zKAsHK6NC2VGd6CwkduN96aaBWS5t"
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <BlynkSimpleEsp32.h>
#define LDR_PIN 36
#define LED_PIN 13

LiquidCrystal_I2C lcd(0x27, 16, 2);

BlynkTimer timer;
int state=LOW;
// Manual LED control from app
BLYNK_WRITE(V1) {
    state = param.asInt();
  digitalWrite(LED_PIN, state);
}

#define WIFI_SSID "Wokwi-GUEST"
#define WIFI_PASSWORD ""
// Defining the WiFi channel speeds up the connection:
#define WIFI_CHANNEL 6

// Send sensor data
void sendData() {
  int ldrValue = analogRead(LDR_PIN);

  // Send to Blynk
  Blynk.virtualWrite(V0, ldrValue);

  // LCD display
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("LDR:");
  lcd.print(ldrValue);

  if (ldrValue > 2000) {
    digitalWrite(LED_PIN, HIGH);
     Blynk.virtualWrite(V1, HIGH); // send LED status to Blynk
    lcd.setCursor(0, 1);
    lcd.print("Dark");
  } else {
    digitalWrite(LED_PIN, LOW);
     Blynk.virtualWrite(V1, LOW); // send LED status to Blynk
    lcd.setCursor(0, 1);
    lcd.print("Light");
  }
}

void setup(void) {
  Serial.begin(115200);

  pinMode(LED_PIN, OUTPUT);

  lcd.init();
  lcd.backlight();

  Blynk.begin(BLYNK_AUTH_TOKEN, WIFI_SSID, WIFI_PASSWORD);

  timer.setInterval(2000L, sendData); // every 2 sec

  WiFi.begin(WIFI_SSID, WIFI_PASSWORD, WIFI_CHANNEL);
  Serial.print("Connecting to WiFi ");
  Serial.print(WIFI_SSID);
  // Wait for connection
  while (WiFi.status() != WL_CONNECTED) {
    delay(100);
    Serial.print(".");
  }
  Serial.println(" Connected!");

  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
}

void loop(void) {
   Blynk.run();
  timer.run();
  delay(500);
}

