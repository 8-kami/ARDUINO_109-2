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
