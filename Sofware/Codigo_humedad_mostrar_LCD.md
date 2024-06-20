```
/*
  LCD 10 X 4 Con Luz De Fondo Azul Con Interfaz I2C
  Humedad_lcd
*/

#include <LiquidCrystal_I2C.h> // Librería para LCD I2C
#include <Wire.h> // Librería requerida para usar SDA y SCL

LiquidCrystal_I2C lcd(0x27, 10, 4); // Inicializar el LCD I2C con dimensiones 10x4

void setup() {
  lcd.begin(10, 4); // Inicializar la LCD 10x4
  lcd.backlight(); // Encender la luz de fondo
  Serial.begin(9600); // Inicializar la comunicación serie
  Serial.println("Valor del sensor de humedad");
}

void loop() {
  // Leer el valor del sensor de humedad
  int humidity = analogRead(A0);

  // Limpiar la pantalla antes de escribir nuevos valores
  lcd.clear();

  // Mostrar mensajes estáticos adaptados al tamaño del LCD
  lcd.setCursor(0, 0); // Iniciar en la columna 0 fila 0
  lcd.print("LCD 10X4"); // Muestra el mensaje "LCD 10X4"
  lcd.setCursor(0, 1); // Iniciar en la columna 0 fila 1
  lcd.print("I2C Interf"); // Muestra el mensaje "I2C Interf" abreviado
  lcd.setCursor(0, 2); // Iniciar en la columna 0 fila 2
  lcd.print("TalosElct"); // Muestra el mensaje abreviado "TalosElct"
  lcd.setCursor(0, 3); // Iniciar en la columna 0 fila 3
  lcd.print("Hum: "); // Muestra el mensaje "Hum: "
  lcd.print(humidity); // Mostrar la lectura del sensor de humedad

  // También enviar los datos a la consola serial
  Serial.print("Lectura: ");
  Serial.println(humidity);

  // Determinar y mostrar el estado del suelo
  if (humidity >= 0 && humidity <= 300) {
    Serial.println("Sensor en suelo seco");
    lcd.print(" Seco");
  } else if (humidity > 301 && humidity <= 700) {
    Serial.println("Sensor en suelo húmedo");
    lcd.print(" Humedo");
  } else if (humidity >= 701) {
    Serial.println("Sensor en agua");
    lcd.print(" Agua");
  }

  delay(1000); // Esperar 1 segundo antes de la siguiente lectura
}
```