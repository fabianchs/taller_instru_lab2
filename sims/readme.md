## Simulaciones (Proteus)
### Etapa 1
En la primera etapa se toma el valor de voltaje de la termocupla, se pasa por un amplificador de instrumentacion y luego se aisla electricamente con un optoacoplador, se busca una ganancia de 1000, por lo tanto, la salida se tiene en el orden de V y no de mV. El esquematico de la primera etapa se muestra a continuacion.

![E1](Etapa1.png)


### Etapa 2

Luego en la segunda etapa mediante un multiplexor y un amplificador en configuracion inversora se selecciona la escala deseada para la salida de la etapa 1 y 2, se escogieron 6 escalas distintas (10V, 1V, 100mV, 10mV, 1mV, 100uV), el esquimatico de la segunda etapa se muestra a continuacion.

![E2](Etapa2.png)


### Etapa 3

En la etapa 4 se usa un optoacoplador para aislar las tierras del circuito, de esta forma se puede conectar la alimentacion y la tierra que proporciona el arduino para tener una lectura mas estable.

![E3](EtapaOpto.png)

La grafica de voltaje por grado centigrado a la salida de la etapa 1 y 2 usada como referencia para poder pasar de digital a analogico nuevamente dentro de la programacion del arduino es la siguiente.

![VXC](VXC.png)

### Etapa 4
En la ultima etapa se conecta el arduino junto con el display lcd como se muestra en la siguiente imagen:

![arldc](Arlcd.png)

### Programacion Arduino
Finalmente, la programacion del arduino es la siguiente.

```arduino
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);  // Establece la dirección I2C del adaptador y el tamaño del LCD (16x2).

void setup() {
  lcd.init();                      // Inicializa el LCD.
  lcd.backlight();                 // Activa la retroiluminación del LCD.
}

void loop() {

    int valorAnalogicoA0 = analogRead(A0);

  // Leer el valor de tensión en el pin analógico A1
  int valorAnalogicoA1 = analogRead(A1);

  // Convertir los valores leídos a voltajes (0-1023 a 0-5V en este caso)
  float voltajeA0 = (valorAnalogicoA0)*0.37;
  float voltajeA1 = (valorAnalogicoA1)*0.37;

  lcd.setCursor(0,0);
  lcd.print("Temp 1: ");
  lcd.print(voltajeA1,2);
  lcd.print(" C");
  lcd.setCursor(0,1);
  lcd.print("Temp 2: ");
  lcd.print(voltajeA0,2);
  lcd.print(" C");


  delay(1000);
}

```