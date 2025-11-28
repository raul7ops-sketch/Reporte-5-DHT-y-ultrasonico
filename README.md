# Reporte-5-DHT-y-ultrasonico
## Introduccion
### En esta practica se realizo la combinacion de los codigos y componenetes de las anteriores dos practicas que hemos realizado. Donde en esta practica se utiliza un DHT11 Y un ESP 22 y los datos arrojados por esos sensores se vera reflejado en una LCD que tambien se realizo en las anteriores practicas.
## Material y equipo a utilizar
- Pantalla LCD
- Sensor DHT 11
- ESP  22
- Software de Wokwi
## Procedimineto
1. Se abrio el software de Wokwi y se agrego la suma de las codigos anteriores que se presenta acontinuacion:
``` #include <LiquidCrystal_I2C.h>
#include "DHTesp.h"
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 2;   //Pin digital 3 para el Echo del sensor
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);
const int DHT_PIN = 15;
DHTesp dhtSensor;

void setup() {
  Serial.begin(9600);//iniciailzamos la comunicación
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
   lcd.init();
  lcd.backlight();
  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
}

void loop()
{

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(2000);     

    TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  delay(2000);
   
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");

  delay(2000);
  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Bienvenidos");
  lcd.setCursor(2, 1);
  lcd.print("modulo 5");
  delay(2000);
  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Distancia: ");
  lcd.print(d);      //Enviamos serialmente el valor de la distancia
  lcd.setCursor(14, 1);
  lcd.print("cm");
  lcd.println();
  delay(2000);
  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Raul Aguilar L.");
  lcd.setCursor(2, 1);
  lcd.print("Ing. Mecanico");
  delay(2000);     
  lcd.clear();
  lcd.setCursor(1, 0);
  lcd.print("Fecha:  ");
  lcd.setCursor(5, 1);
  lcd.print("22/11/25");
       //Hacemos una pausa de 100ms
}
```
2. Se conecatan los dispositivos y sensores de la siguiente manera:
![](https://github.com/ossajjasso-arch/Practica-reporte/blob/main/images.jpg?raw=true)
