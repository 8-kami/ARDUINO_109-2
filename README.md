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
