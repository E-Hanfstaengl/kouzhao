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
### 消毒程序代码
```markdown
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



Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/E-Hanfstaengl/kouzhao/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
