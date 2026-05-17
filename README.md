# summer-internship
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <DHT.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

// Change this pin based on your board
#define DHTPIN 4      // ESP8266 D2/GPIO4 or ESP32 GPIO4
#define DHTTYPE DHT11 // Change to DHT22 if using DHT22

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED not found");
    while (true);
  }

  display.clearDisplay();
  display.setTextColor(WHITE);

  display.setTextSize(1);
  display.setCursor(10, 10);
  display.println("Smart Weather");
  display.setCursor(25, 25);
  display.println("Station");
  display.display();

  delay(2000);
}

void loop() {
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();

  if (isnan(temperature) || isnan(humidity)) {
    display.clearDisplay();
    display.setTextSize(1);
    display.setCursor(10, 20);
    display.println("Sensor Error!");
    display.display();
    delay(2000);
    return;
  }

  display.clearDisplay();

  display.setTextSize(1);
  display.setCursor(15, 0);
  display.println("Weather Station");

  display.drawLine(0, 12, 128, 12, WHITE);

  display.setTextSize(1);
  display.setCursor(0, 22);
  display.print("Temperature:");

  display.setTextSize(2);
  display.setCursor(0, 34);
  display.print(temperature);
  display.print(" C");

  display.setTextSize(1);
  display.setCursor(0, 55);
  display.print("Humidity: ");
  display.print(humidity);
  display.print(" %");

  display.display();

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" C  Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  delay(2000);
}
