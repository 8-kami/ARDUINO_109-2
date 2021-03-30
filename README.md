# ARDUINO_109-2
ARDUINOmemo </p>
<h3> 學習ARDUINO 的過程 part2 </h3>

__第一個程式　功能：七段顯示器由0亮至9__ </p>
```c++
int a[10][7]={
              {1,1,1,1,1,1,0},
              {0,1,1,0,0,0,0},
              {1,1,0,1,1,0,1},
              {1,1,1,1,0,0,1},
              {0,1,1,0,0,1,1},
              {1,0,1,1,0,1,1},
              {1,0,1,1,1,1,1},
              {1,1,1,0,0,1,0},
              {1,1,1,1,1,1,1},
              {1,1,1,1,0,1,1}
              };
void setup() {
for(int i=2; i<13; i++)
  pinMode(i, OUTPUT);
}

void loop() {
digitalWrite(9,HIGH);
for(int j=0; j<10; j++){
for(int i=2; i<9; i++)
  digitalWrite(i,a[j][i-2]);
  delay(500);}
}
```

電路圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210302_1.jpg) </p>


__功能：d陣列控制七段顯示器所顯示之數字(delay ver.)__ </p>
```c++
int d[4]={8, 4, 6, 1};
float c;
int b[4][4]={
              {1, 0, 0, 0}, 
              {0, 1, 0, 0},
              {0, 0, 1, 0},
              {0, 0, 0, 1}
            };
int a[10][7]={
              {1,1,1,1,1,1,0},
              {0,1,1,0,0,0,0},
              {1,1,0,1,1,0,1},
              {1,1,1,1,0,0,1},
              {0,1,1,0,0,1,1},
              {1,0,1,1,0,1,1},
              {1,0,1,1,1,1,1},
              {1,1,1,0,0,1,0},
              {1,1,1,1,1,1,1},
              {1,1,1,1,0,1,1}
              };
void setup() {
for(int i=2; i<13; i++)
  pinMode(i, OUTPUT);
  c = millis(); //開機到下指令的那一個時間點，所經過的毫秒數
  Serial.begin(9600);
}

void loop() {
for (int j=0;j<4;j++){
    for (int k=9;k<13;k++)
      digitalWrite(k,b[j][k-9]); //com
      
    for (int i=2;i<9;i++)
      digitalWrite(i,a[d[j]][i-2]); //7seg
      delay(5);
      
  };
}
```

結果圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210302_2.jpg) </p>

__2021.3.2__ </p>

__第二個程式　功能：操控LCD顯示器__ </p>
功能包括
1自製圖形
2顯示blink閃爍及Cursor底線
3按鈕控制圖形向右移
4按鈕控制圖形向左移
```c++

// include the library code:
#include <LiquidCrystal.h>

// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
byte smiley[8] = {
  B00000,
  B10001,
  B00000,
  B00000,
  B11111,
  B01110,
  B00000,
};

void setup() {
  // set up the LCD's number of columns and rows:
  lcd.begin(16, 2);
  // Print a message to the LCD.
  lcd.createChar(0, smiley);
  lcd.begin(16, 2);  
  lcd.write(byte(0));
  lcd.setCursor(0, 1); 
  lcd.blink(); 
  pinMode(6, INPUT);
  pinMode(7, INPUT);
  digitalWrite(6, HIGH);  
  digitalWrite(7, HIGH);  
  lcd.noCursor();delay(500);lcd.cursor();delay(500);

}

void loop() {

  if(digitalRead(6)==LOW){
      while(digitalRead(6)==LOW);{delay(10);}
      lcd.scrollDisplayRight();
    }
  if(digitalRead(7)==LOW){
      while(digitalRead(7)==LOW);{delay(10);}
      lcd.scrollDisplayLeft();
    }  
  

}


```


結果圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210309_0.jpg) </p>
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210309_1.jpg) </p>
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210309_2.jpg) </p>

__2021.3.9__ </p>

__第三個程式　功能：以HT6751馬達操控風扇，馬力分為三段__ </p>
*各馬力之間有停頓* </p>
```c++
int a;
void setup() {
  pinMode(5,OUTPUT); //IN1
  pinMode(6,OUTPUT); //IN2
  
}

void loop() {
  for (int i=0;i<3;i++)
  {
    a= a + 100;
    if (a > 255) {a=255;};
    analogWrite(5,HIGH);
    analogWrite(6,HIGH);
    delay(3000);
    analogWrite(5,a);
    analogWrite(6,LOW);
    delay(3000);
  };
  if (a >= 255) {a=0;};
  
  /*
  digitalWrite(5,HIGH); //正轉
  digitalWrite(6,LOW);
  delay(5000);
  digitalWrite(5,LOW); //逆轉
  digitalWrite(6,HIGH);
  delay(5000);
  digitalWrite(5,LOW); //STANDBY
  digitalWrite(6,LOW);
  delay(5000);
  digitalWrite(5,HIGH); //煞車
  digitalWrite(6,HIGH);
  delay(5000);
  */
}
```
電路圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210316_0.gif) </p>

*各馬力之間無停頓* </p>
```c++
int a;
void setup() {
  pinMode(5,OUTPUT); //IN1
  pinMode(6,OUTPUT); //IN2
  
}

void loop() {
  for (int i=0;i<3;i++)
  {
    a= a + 95;
    if (a > 255) {a=255;};

    analogWrite(5,a);
    analogWrite(6,LOW);
    delay(5000);
  };
  if (a >= 255) {a=0;};

}

```
電路圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210316_1.gif) </p>

*按鈕控制馬力、風扇開關* </p>
```c++
int a=100;
int b=0;
void setup() {
  Serial.begin(9600);
  pinMode(2,INPUT);
  pinMode(3,INPUT);
  pinMode(4,INPUT);
  digitalWrite(4, HIGH);  
  digitalWrite(2, HIGH);  
  digitalWrite(3, HIGH);  
  pinMode(5,OUTPUT); //IN1
  pinMode(6,OUTPUT); //IN2
  
}
void motor(int a){
    analogWrite(5,a);
    analogWrite(6,LOW);
  }
void loop() {
  if (digitalRead(4)==LOW)
  {
    while (digitalRead(4)==LOW);
    b=(b+1)%2;
  }
  switch(b)
  {
    case 0:
    {motor(0);break;}
    case 1:
    {motor(a);break;}
  }
  if (digitalRead(2)==LOW)
  {
    while (digitalRead(2)==LOW);
    a= a + 35; 
    if (a >= 255) {a=255;}
  }
  if (digitalRead(3)==LOW)
  {
    while (digitalRead(3)==LOW);
    a= a - 35; 
    if (a <= 100) {a=100;}
  }
  Serial.println(a);
}
```
電路圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210316_2.gif) </p>
__2021.3.16__ </p>

__第四個程式　功能：以溫溼度感測器DHT22操控嗡鳴器 __ </p>
```c++
// DHT Temperature & Humidity Sensor
// Unified Sensor Library Example
// Written by Tony DiCola for Adafruit Industries
// Released under an MIT license.

// REQUIRES the following Arduino libraries:
// - DHT Sensor Library: https://github.com/adafruit/DHT-sensor-library
// - Adafruit Unified Sensor Lib: https://github.com/adafruit/Adafruit_Sensor
int a;
#include <Adafruit_Sensor.h>
#include <DHT.h>
#include <DHT_U.h>

#define DHTPIN 2     // Digital pin connected to the DHT sensor 
// Feather HUZZAH ESP8266 note: use pins 3, 4, 5, 12, 13 or 14 --
// Pin 15 can work but DHT must be disconnected during program upload.

// Uncomment the type of sensor in use:
//#define DHTTYPE    DHT11     // DHT 11
#define DHTTYPE    DHT22     // DHT 22 (AM2302)
//#define DHTTYPE    DHT21     // DHT 21 (AM2301)

// See guide for details on sensor wiring and usage:
//   https://learn.adafruit.com/dht/overview

DHT_Unified dht(DHTPIN, DHTTYPE);

uint32_t delayMS;

void setup() {
  Serial.begin(9600);
  // Initialize device.
  dht.begin();
  Serial.println(F("DHTxx Unified Sensor Example"));
  // Print temperature sensor details.
  sensor_t sensor;
  dht.temperature().getSensor(&sensor);

  // Set delay between sensor readings based on sensor details.
  delayMS = sensor.min_delay / 1000;
}

void loop() {
  // Delay between measurements.
  delay(delayMS);
  // Get temperature event and print its value.
  sensors_event_t event;
  dht.temperature().getEvent(&event);
  if (isnan(event.temperature)) {
    Serial.println(F("Error reading temperature!"));
  }
  else {
    Serial.print(F("Temperature: "));
    Serial.print(event.temperature);
    Serial.println(F("°C"));
    a = event.temperature;
  }
  // Get humidity event and print its value.
  dht.humidity().getEvent(&event);
  if (isnan(event.relative_humidity)) {
    Serial.println(F("Error reading humidity!"));
  }
  else {
    Serial.print(F("Humidity: "));
    Serial.print(event.relative_humidity);
    Serial.println(F("%"));
  }

  if(a>=26)
  {
    tone(3,500);
    delay(500);
    noTone(3);
    delay(500);
    Serial.print(a);
  }
}
```

電路圖如下：
![image](https://github.com/8-kami/ARDUINO_109-2/blob/main/20210330_0.jpg) </p>
