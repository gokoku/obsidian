#vscode #esp32

---
2021-12-15

# PlatformIO IDE で Arduino 出来るらしい

![[Pasted image 20211215101740.png]]


## 起動

コマンド + shift コマンドパレットで

![[Pasted image 20211215101842.png]]

PIO Home

![[Pasted image 20211215102136.png]]

### Arduino IDE からのインポート

#arduino   Arduino IDE を Mac に入れる]]

import Arduino Project で board は ESP32 FM DevKit にした。

![[Pasted image 20211215102934.png]]


### 新規

コマンド + shift コマンドパレットで

![[Pasted image 20211215101842.png]]

PIO Home

+New Project

![[Pasted image 20211215105301.png]]

* Name: Sample
* Board ESP32 FM DevKit(Unkown)
* Framework: Arduino
* Location : Users/george/ PlatformIO    <-- 作った

Sample プロジェクトが作られた。c++ だ。

Sample/src/main.cpp

```cpp
#define <Arduino.h>

void setup() {
  // put your setup code here, to run once:
}

void loop() {
  // put your main code here, to run repeatedly:
}
```

Arduino IDE と違って、#define <Arduino.h> が必要らしい。

Sample/src/main.cpp

```cpp
#define <Arduino.h>

#define LED_PIN 13

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  delay(1000);
  digitalWrite(LED_PIN, LOW);
  delay(2000);
}
```


## ビルド

![[Pasted image 20211215104251.png]]

コマンドパレット PlatformIo: Build

## アップロード

矢印

![[Pasted image 20211215104453.png]] 

PlatformIo: upload

出来てしまった。

## Doc

https://docs.platformio.org/en/latest/what-is-platformio.html

# MQTT サンプルを移植してみる

Arduino IDE で教材で MQTT がある。これを新規からライブラリをインストールしてなんとか出来るか。


### PUbSubClient ライブラリインストール

ビルドすると PubSubClient.h ないと言われるので、ライブラリインストール

![[Pasted image 20211217161725.png]]

どのプロジェクトに入れるか指定して追加する。

### DHT11ライブラリインストール

![[Pasted image 20211217162213.png]]

DHT sensor Library の方を入れる。

Adafruit_Sensor.h が無いと言われたので、

Adafuit Unified Sensor を入れる。

![[Pasted image 20211217162817.png]]

ビルド & ライト　OK!!!


```cpp
#define <Arduino.h>
#define "WiFi.h"
#define <PubSubClient.h>
#define <DHT.h>

#define N_AVE_TIME 50

const int PIN_DHT = 33;
const int PIN_PTrB = 32;
const int PIN_PTrY = 34;
const int PIN_PTrR = 35;
const int PIN_LED = 25;

DHT dht(PIN_DHT, DHT11);
char *ssidC = "peopleTONO_G_own";
char *passC = "pmorioka7481";
char *servC = "192.168.20.103";

const char *gcTopic = "myTopic";
String gsClientId;

WiFiClient myWifiClient;
const int myMqttPort = 1883;
PubSubClient myMqttClient(myWifiClient);

void selfRestartProcess(void);

#define NUM_1SEC_MICROSEC (1000000)
#define NUM_SEC_TIMER (10)
#define NUM_TIMER_MICROSEC (NUM_SEC_TIMER*NUM_1SEC_MICROSEC)
hw_timer_t *handleTimer2 = NULL;
portMUX_TYPE timerMux2 = portMUX_INITIALIZER_UNLOCKED;
volatile int gviCountWatchDog = 0;
#define NUM_MAX_WATCHDOG (6) // 60sec

float gfBlueAveragedVoltage;
float gfYellowAveragedVoltage;
float gfRedAveragedVoltage;

uint32_t gu32Count;

void IRAM_ATTR irq_Timer2()
{
  portENTER_CRITICAL_ISR(&timerMux2);
  gviCountWatchDog++;
  if (gviCountWatchDog > NUM_MAX_WATCHDOG)
  {
    selfRestartProcess();
  }
  portEXIT_CRITICAL_ISR(&timerMux2);
}

void init_TimerInterrupt2()
{
  gviCountWatchDog = 0;
  handleTimer2 = timerBegin(2, 80, true); // Num_timer, counter per clock
  timerAttachInterrupt(handleTimer2, &irq_Timer2, true);
  timerAlarmWrite(handleTimer2, NUM_TIMER_MICROSEC, true); //[us] per 80 clock @ 80MHz
  timerAlarmEnable(handleTimer2);
}

void selfRestartProcess(void)
{
  //memory copy to flash
  ESP.restart(); //reboot
}

void initWifiClient(void)
{
  Serial.print("Connecting to ");
  Serial.println(ssidC);
  uint16_t tmpCount = 0;

  WiFi.begin(ssidC, passC);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
    tmpCount++;
    if (tmpCount > 128)
    {
      Serial.println("WiFi connect fail");
      return;
    }
  }
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void connectMqtt()
{
  myMqttClient.setServer(servC, myMqttPort);
  while( ! myMqttClient.connected() )
  {
    Serial.print("Connecting to MQTT Broker... ");
    gsClientId = "ESP32_" + String(random(0xffff), HEX);
    if (myMqttClient.connect(gsClientId.c_str()))
    {
      Serial.println("connected");
    }
    else
    {
      Serial.print("failed, rc=");
      Serial.print(myMqttClient.state());
      Serial.println(" try again in 5 seconds");
      delay(5000);
    }
  }
}

void doPubMqtt(char *cValue)
{
  if (myMqttClient.connected())
  {
    if (myMqttClient.connect(gsClientId.c_str()))
    {
      Serial.println("reconnected to MQTT broker");
    }
    else
    {
      Serial.print("failure to reconnect to MQTT broker");
      return;
    }
    // myMqttClient.loop();
    boolean bFlagSucceed = myMqttClient.publish(gcTopic, cValue);
    // free(cValue);
    if (bFlagSucceed)
    {
      for( int i=0; i<5; i++)
      {
        digitalWrite(PIN_LED, HIGH);
        delay(20);
        digitalWrite(PIN_LED, LOW);
        delay(20);
      }
      Serial.println("done: ");
      Serial.println(cValue);
    }
    else
    {
      Serial.println("publish failed");
    }
  }
}

void LampScan()
{
  float fBCurrentVoltage = analogRead(PIN_PTrB) / (float)4096 * 3.3;
  float fYCurrentVoltage = analogRead(PIN_PTrY) / (float)4096 * 3.3;
  float fRCurrentVoltage = analogRead(PIN_PTrR) / (float)4096 * 3.3;
  gfBlueAveragedVoltage = fBCurrentVoltage;
  gfYellowAveragedVoltage = fYCurrentVoltage;
  gfRedAveragedVoltage = fRCurrentVoltage;
}

void setup() {
  Serial.begin(115200);
  Serial.println("DHT11");
  pinMode(PIN_LED, OUTPUT);
  dht.begin();
  gviCountWatchDog = 0;
  init_TimerInterrupt2();
  initWifiClient();
  connectMqtt();
}

void loop() {
  portENTER_CRITICAL(&timerMux2);
  gviCountWatchDog = 0;
  portEXIT_CRITICAL(&timerMux2);

  bool isFahrenheit = false;
  float percentHumidity = dht.readHumidity();
  float temperature = dht.readTemperature( isFahrenheit );

  if( isnan(percentHumidity) || isnan(temperature) )
  {
    Serial.println("ERROR");
    return;
  }
  if( WiFi.status() != WL_CONNECTED )
  {
      initWifiClient();
  }

  //パトライト各色初期値(平均化処理前段)
  gfBlueAveragedVoltage = 1.0;
  gfYellowAveragedVoltage = 1.0;
  gfRedAveragedVoltage = 1.0;

  //パトライトスキャン青B,黄Y,赤R　平均化処理1秒ディレイ
  LampScan();
  // 平均化カウンタリセット
  gu32Count = 0;
  // 追加ディレイインターバル1000 = 1秒
  delay(500);

  //heatIndex: 体感温度
  float heatIndex = dht.computeHeatIndex(temperature, percentHumidity, isFahrenheit);

  String json = "{";
  json += "\"devices\": \"ESP32\"";
  json += ",";
  json += "\"envsensor\":";
  json += "{";
  json += "\"temperature\":";
  json += String(temperature, 1);
  json += ",";
  json += "\"humidity\":";
  json += String(percentHumidity, 1);
  json += ",";
  json += "\"heatindex\":";
  json += String(heatIndex, 1);
  json += "}";
  json += ",";
  json += "\"lampsensor\":";
  json += "{";
  json += "\"blue\":";
  json += String(gfBlueAveragedVoltage, 1);
  json += ",";
  json += "\"yellow\":";
  json += String(gfYellowAveragedVoltage, 1);
  json += ",";
  json += "\"red\":";
  json += String(gfRedAveragedVoltage, 1);
  json += "}";
  json += "}";

  char *cValue;
  cValue = (char *)malloc(json.length() + 1);
  json.toCharArray(cValue, json.length() + 1);

  doPubMqtt(cValue);
  free(cValue);

  portENTER_CRITICAL(&timerMux2);
  gviCountWatchDog = 0;
  portEXIT_CRITICAL(&timerMux2);
  delay(1500);
}
```

