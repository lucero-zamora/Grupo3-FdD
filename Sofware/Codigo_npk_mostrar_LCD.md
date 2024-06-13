```
/*
  LCD 10 X 4 Con Luz De Fondo Azul Con Interfaz I2C
  NPK_lcd
*/

#include <LiquidCrystal_I2C.h> // Librería para LCD I2C
#include <Wire.h> // Librería requerida para usar SDA y SCL

LiquidCrystal_I2C lcd(0x27, 10, 4); // Inicializar el LCD I2C con dimensiones 10x4

void setup() {
  lcd.begin(10, 4); // Inicializar la LCD 10x4
  lcd.backlight(); // Encender la luz de fondo
  Serial.begin(9600); // Inicializar la comunicación serie
  Serial.println("Sensor NPK");
}

void loop() {
  // Leer los valores de los sensores NPK
  int nitrogen = analogRead(A0);
  int phosphorus = analogRead(A1);
  int potassium = analogRead(A2);

  // Limpiar la pantalla antes de escribir nuevos valores
  lcd.clear();

  // Mostrar valores de NPK en la pantalla LCD
  lcd.setCursor(0, 0); // Iniciar en la columna 0 fila 0
  lcd.print("N: ");
  lcd.print(nitrogen);

  lcd.setCursor(0, 1); // Iniciar en la columna 0 fila 1
  lcd.print("P: ");
  lcd.print(phosphorus);

  lcd.setCursor(0, 2); // Iniciar en la columna 0 fila 2
  lcd.print("K: ");
  lcd.print(potassium);

  // También enviar los datos a la consola serial
  Serial.print("Nitrógeno: ");
  Serial.println(nitrogen);

  Serial.print("Fósforo: ");
  Serial.println(phosphorus);

  Serial.print("Potasio: ");
  Serial.println(potassium);

  delay(1000); // Esperar 1 segundo antes de la siguiente lectura
}
```
