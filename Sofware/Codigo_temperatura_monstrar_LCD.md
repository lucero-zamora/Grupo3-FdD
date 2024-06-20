```

/*
  LCD 10 X 4 Con Luz De Fondo Azul Con Interfaz I2C
  Temperatura_lcd
*/

#include <LiquidCrystal_I2C.h> // Librería para LCD I2C
#include <Wire.h> // Librería requerida para usar SDA y SCL

LiquidCrystal_I2C lcd(0x27, 10, 4); // Inicializar el LCD I2C con dimensiones 10x4

void setup() {
  lcd.begin(10, 4); // Inicializar la LCD 10x4
  lcd.backlight(); // Encender la luz de fondo
  Serial.begin(9600); // Inicializar la comunicación serie
  Serial.println("Sensor de Temperatura LM35");
}

void loop() {
  // Leer el valor del sensor de temperatura LM35
  int sensorValue = analogRead(A0);
  
  // Convertir el valor a temperatura en grados Celsius
  float voltage = sensorValue * (5.0 / 1023.0);
  float temperatureC = voltage * 100.0;

  // Limpiar la pantalla antes de escribir nuevos valores
  lcd.clear();

  // Mostrar mensajes estáticos adaptados al tamaño del LCD
  lcd.setCursor(0, 0); // Iniciar en la columna 0 fila 0
  lcd.print("Temp:"); // Muestra el mensaje "Temp:"
  lcd.setCursor(6, 0); // Iniciar en la columna 6 fila 0
  lcd.print(temperatureC); // Mostrar la temperatura en grados Celsius
  lcd.print(" C");

  // También enviar los datos a la consola serial
  Serial.print("Temperatura: ");
  Serial.print(temperatureC);
  Serial.println(" C");

  delay(1000); // Esperar 1 segundo antes de la siguiente lectura
}

```