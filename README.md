# ARDUINO_109-1 
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
