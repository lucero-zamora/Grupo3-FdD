```

#include <ESP8266WiFi.h>
#include <DHT.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Configuración del sensor DHT
#define DHTPIN D4     // Pin del sensor DHT
#define DHTTYPE DHT11  // Tipo de sensor DHT (DHT11 o DHT22)

DHT dht(DHTPIN, DHTTYPE);

// Configuración del LCD I2C
LiquidCrystal_I2C lcd(0x27, 16, 2);  // Dirección I2C y dimensiones del LCD (16x2)

// Configuración de WiFi
const char* ssid = "TuSSID";
const char* password = "TuPassword";

void setup() {
  Serial.begin(9600);
  dht.begin();
  lcd.begin(16, 2);
  lcd.backlight();

  // Conectar a WiFi
  WiFi.begin(ssid, password);
  Serial.println("Conectando a WiFi...");
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Conexión WiFi establecida!");
  }
}

void loop() {
  // Lectura de humedad y temperatura
  float humedad = dht.readHumidity();
  float temperatura = dht.readTemperature();

  // Lectura de NPK (simulado, valores analógicos)
  int nitrogeno = analogRead(A0);
  int fosforo = analogRead(A1);
  int potasio = analogRead(A2);

  // Mostrar en LCD (opcional)
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperatura);
  lcd.print("C");

  lcd.setCursor(0, 1);
  lcd.print("Humedad: ");
  lcd.print(humedad);
  lcd.print("%");

  // Enviar datos por WiFi
  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("Enviando datos...");
    // Ejemplo de envío a un servicio HTTP (ajustar la URL según el destino)
    String data = "temperatura=" + String(temperatura) +
                  "&humedad=" + String(humedad) +
                  "&nitrogeno=" + String(nitrogeno) +
                  "&fosforo=" + String(fosforo) +
                  "&potasio=" + String(potasio);
                  
    // Ejemplo de enviar datos a un servidor HTTP
    HTTPClient http;
    http.begin("http://tu-servidor.com/receptor");  // URL del servidor
    http.addHeader("Content-Type", "application/x-www-form-urlencoded");

    int httpResponseCode = http.POST(data);  // Enviar los datos

    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println(httpResponseCode);
      Serial.println(response);
    } else {
      Serial.print("Error en la solicitud HTTP: ");
      Serial.println(httpResponseCode);
    }

    http.end();
  }

  delay(5000);  // Esperar 5 segundos antes de la siguiente lectura
}

```
