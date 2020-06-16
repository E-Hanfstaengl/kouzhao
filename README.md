## 计时消毒口罩盒

### 团队介绍与分工：
**create小组，成员：** 常温新，姜生新，李响，唐志豪，张凯

**电路设计：** 张凯

**3d建模：** 唐志豪

**编程：** 李响

**app界面：** 常温新

**实物组装：** 姜生新

### 设计概念：
疫情期间，规范佩戴口罩是保护身体健康的必要手段，但口罩佩戴时间长短难以把控、消毒方式难以达标使得口罩的防护效果受到一定程度的影响。本小组设计的计时消毒口罩盒旨在提供储存、记录佩戴时间并达到在口罩的有效防护期6小时通过手机app提醒佩戴者更换口罩、对口罩进行紫外线消毒等功能，使口罩的更换和再次利用更加便利。
### 材料清单：
|名称|数量|备注|
|:---|:---|:---|
|紫外LED灯|4|用于消毒|
|红色LED灯|1|提示口罩使用时限已到|
|黄色LED灯|1|提示准备消毒或消毒进行中|
|绿色LED灯|1|提示消毒完成或口罩还能继续使用|
|蜂鸣器|1|提示消毒完成以及口罩使用时限已到|
|LCD显示板|1|显示时间|
|Arduino板|1|装载程序|
|电池|2|
|电线|若干|
|纸板|若干|搭建消毒盒主体|
### 程序代码（含消毒部分）
```
int purplePin = 2 ;
int yellowPin = 4 ;
int greenPin = 5 ;
int redPin = 7 ;

int speakPin = 6 ;

int pressPin = 3 ;
int clickPin = 8 ;

int judge = 1 ;
int kg = 1;
int timerOne = 0 ;
int timerTwo = 0 ;
int timerThree = 0 ;
int limit = 1000*60*60*6;

void setup() {
pinMode(purplePin, OUTPUT);
pinMode(redPin, OUTPUT);
pinMode(yellowPin, OUTPUT);
pinMode(greenPin, OUTPUT);
pinMode(speakPin, OUTPUT);
pinMode(pressPin, INPUT_PULLUP);
pinMode(clickPin, INPUT_PULLUP);
}

void loop() {

if ( digitalRead(pressPin) == LOW){
  judge = 0;
  digitalWrite(yellowPin, HIGH);
  delay(30000);
}

if (judge == 0) {
  digitalWrite(purplePin, HIGH);
  digitalWrite(yellowPin, HIGH);
  delay(1000*60*30);
  digitalWrite(purplePin, LOW);
  digitalWrite(yellowPin, LOW);
  digitalWrite(speakPin, HIGH);
  digitalWrite(greenPin, HIGH);
  delay(10000);
  digitalWrite(speakPin, LOW);
  digitalWrite(greenPin, LOW);
  judge = 1;
}

if ( digitalRead(clickPin) == LOW){
  if ( kg == 1) {
    kg = 0 ;
    timerOne = millis();
  }else {
      digitalWrite(greenPin, HIGH);
      delay(10000);
      digitalWrite(greenPin, LOW);
      kg = 1;
     }  
}

if (kg == 0){
 timerTwo = millis();
 timerThree = timerTwo - timerOne;
  if (timerThree >= limit){
    digitalWrite(redPin, HIGH);
    digitalWrite(speakPin, HIGH);
    delay(10000);
    digitalWrite(redPin, LOW);
    digitalWrite(speakPin, LOW);
    kg = 1;
  }
}

}  
```
###程序代码（含显示时间部分）
```
#include<LiquidCrystal.h>
#define buzzer 4
#define BUTTON 5
#define LED 9//消毒完毕提示灯
LiquidCrystal lcd(2,3,12,11,10,13);
int a=0;//时间设定
int b=0;
int c=0;
int val=0;//储存紫外灯按钮状态
int old_val=0;
int state=0;//0紫外灯关闭1打开
void setup() {
  lcd.begin(16,2);
  lcd.print("Youhaveusedfor:");
  pinMode(buzzer,OUTPUT);
  for(int UVLED=6;UVLED<=8;UVLED++){
   pinMode(UVLED,OUTPUT);
   }
   pinMode(LED,OUTPUT);
  pinMode(BUTTON,INPUT);
}
void loop(){
lcd.setCursor(0,1);//以下为计时 显示以及蜂鸣器提醒
  if(millis()<=216000000){
  if(millis()<=60000){a=0;b=0;c=millis()/1000;}
  if((millis()>60000)&&(millis()<=3600000)){
    a=0;
    b=int (millis()/60000);
    c=(millis()-(60000*b))/1000;
  }
  if((millis()>3600000)){
    a=int (millis()/3600000);
    b=int ((millis()-a*3600000)/60000);
    c=(millis()-(3600000*a)-(60000*b))/1000;
  }
  lcd.print(a);
  lcd.setCursor(2,1);
  lcd.print(":");
  lcd.setCursor(3,1);
  lcd.print(b);
  lcd.setCursor(5,1);
  lcd.print(":");
  lcd.setCursor(6,1);
    lcd.print(c);}
  else{lcd.print("TIME IS UP");
      digitalWrite(buzzer,HIGH);}//以下为按键控制紫外灯亮起
      val=digitalRead(BUTTON);//读取按钮输入并存储，检查变化情况
  if((val==HIGH)&&(old_val==LOW)){
    state=1-state;
    delay(1000);
    }
    old_val=val;//暂存
    if(state==1){
      for(int UVLED=6;UVLED<=8;UVLED++){
       digitalWrite(UVLED,HIGH);}
       digitalWrite(LED,LOW);
        delay(900000);//消毒时间15分钟
        for(int UVLED=6;UVLED<=8;UVLED++){
       digitalWrite(UVLED,LOW);}
       digitalWrite(LED,HIGH);
       delay(300000);
       }
      else{
        for(int UVLED=6;UVLED<=8;UVLED++){
       digitalWrite(UVLED,LOW);}
       delay(600);
        }}
        ```
[TIMER](https://github.com/E-Hanfstaengl/a-new-repository/blob/master/%E4%BB%A3%E7%A0%81%E8%AE%A1%E6%97%B6%E5%92%8C%E6%B6%88%E6%AF%92%26%E7%AE%80%E6%98%93%E5%AE%9E%E7%89%A9%E6%BC%94%E7%A4%BA/TIMER.brd)
>**简易实物演示**
>![image]()
>![image]()
>[uvled.mp4]()

消毒盒子的紫外线消毒和口罩佩戴提醒功能，(暂时通过盒子本身蜂鸣器提醒用户)经查询可知
1. 对眼睛皮肤有害，解决方法：把待消毒物品放入盒子，关闭盒子后按开关，紫外灯亮起，开始消毒。
消毒过程中不能打开盒子，消毒完毕紫外灯关闭，待盒子上绿色提示灯亮起，用户将已消毒物品取出。
之后用户如果想对其他物品消毒，需要在绿灯亮起时，放入待消毒物品。
2. 紫外灯在250-260nm波长能起到较好杀菌作用，选用个波长275nm辐射通量5mw的紫外LED，照射剂量为为300J/m2时才能做到有效杀菌，
根据盒子大小估算得出13min左右可做到消毒，因此设定消毒时长为15min

### 电路图
![image]()

### 三维渲染图
![总装 v5(底+电子元件）.png]()
![总装 v5（中层）.png]()
![总装 v5（盖：俯视）.png	]()
![总装 v6（整体：不透明）.png]()

### app界面

### 实物展示

### 相关信息
[凿物网展示页面](https://zaowu.fun/p/5ee777be9c5fec674b69016f)
