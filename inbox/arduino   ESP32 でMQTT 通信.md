#arduino #esp32

---
2021-12-10

# ESP32 でMQTT 通信

![[ESP32.png]]

Arduino IDE で ESP32 でコードを書き込む。

メニューバーの「ツール」--> 「ボード」 --> 「ESP32 Dev Module」

Flash Mode : QIO
Flash Frequency : 80MHz
Flash Size : 4Mb
Upload Speed : 921600
Core Debug Level : なし
シリアルポート : /dev/ttyUSB0 に変更

## Lチカ

13 番ピンに LED 短い方を GND

```c
#define LED_PIN 13

void setup() {
   pinMode(LED_PIN, OUTPUT);
}

void loop() {
   digitalWrite(LED_PIN, HIGH);
   delay(1000);
   digitalWrite(LED_PIN, LOW);
   delay(1000);
}
```

マイコンに書き込むで動く!!!
なんてお手軽なんだ!!


## MQTT パブリッシュ

センサーからデータを取得して、1定時間でパブリッシュする。

Pub/Sub ブローカーで受信して登録しているサブクライバが受け取る。

```c
#define "WiFi.h"
#define <PubSubClient.h> // https://github.com/knolleary/pubsubclient
#define <DHT.h>
#define N_AVE_TIME 50
const int PIN_DHT = 33;
const int PIN_PTrB = 32;
const int PIN_PTrY = 34;
const int PIN_PTrR = 35;
const int PIN_LED = 25;
DHT dht( PIN_DHT, DHT11 );
char *ssidC="xxxxxxxxxxxx"; // WiFi SSID
char *passC="xxxxxxxxxxxx"; // WiFi Password
char *servC="192.168.xx.xx"; // MQTT Server IP
const char* gcTopic = "myTopic";
String gsClientId;
WiFiClient myWifiClient;
const int myMqttPort = 1883;
PubSubClient myMqttClient(myWifiClient);
#define NUM_1SEC_MICROSEC (1000000)
#define NUM_SEC_TIMER (10)
#define NUM_TIMER_MICROSEC (NUM_SEC_TIMER*NUM_1SEC_MICROSEC)
hw_timer_t * handleTimer2 = NULL; 
portMUX_TYPE timerMux2 = portMUX_INITIALIZER_UNLOCKED; 
volatile int gviCountWatchDog=0;
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
}//irq_Timer02()
void init_TimerInterrupt2()
{
 gviCountWatchDog =0;
 handleTimer2 = timerBegin(2, 80, true); // Num_timer ,  counter per clock 
 timerAttachInterrupt(handleTimer2, &irq_Timer2, true);
 timerAlarmWrite(handleTimer2, NUM_TIMER_MICROSEC, true); //[us] per 80 clock @ 80MHz
 timerAlarmEnable(handleTimer2);
}//init_TimerInterrupt2()
void selfRestartProcess(void)
{
 //memory copy to flash
 ESP.restart(); // reboot
}//selfRestartProcess()
void initWifiClient(void){
 Serial.print("Connecting to ");
 Serial.println(ssidC);
 uint16_t tmpCount =0;
 
 WiFi.begin( ssidC, passC);
 while (WiFi.status() != WL_CONNECTED) {
 delay(500);
 Serial.print(".");
 tmpCount++;
 if(tmpCount>128)
 {
 //gFlagWiFiConnectFailure = true;
 Serial.println("failed  ");
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
 Serial.println("Connecting to MQTT Broker...");
 gsClientId = "ESP32-" + String(random(0xffff),HEX) ;
 if ( myMqttClient.connect(gsClientId.c_str()) )
 {
 Serial.println("connected"); 
 }
 }
}
void doPubMqtt(char * cValue)
{
 if(!myMqttClient.connected())
 { 
 if (myMqttClient.connect(gsClientId.c_str()) )
 {
 Serial.println("reconnected to MQTT broker"); 
 }
 else
 {
 Serial.println("failure to reconnect to MQTT broker");
 return;
 }
 }
 //myMqttClient.loop();
 boolean bFlagSucceed=myMqttClient.publish(gcTopic,cValue);
 //free(cValue);
 if(bFlagSucceed)
 {
 for( int i=0; i<5; i++){
 digitalWrite(PIN_LED, HIGH);
 delay(20);
 digitalWrite(PIN_LED, LOW);
 delay(20);
 }
 Serial.print("done: ");
 Serial.println(cValue);
 }
 else
 Serial.println("failure");
 // run to check: mosquitto_sub -t myTopic
}
void LampScan(){
 //平均値処理コメントアウト中(瞬時値出力に)
 //while( gu32Count < N_AVE_TIME ) {
 float fBCurrentVoltage = analogRead(PIN_PTrB) / (float)4096 * 3.3 ;
 float fYCurrentVoltage = analogRead(PIN_PTrY) / (float)4096 * 3.3 ;
 float fRCurrentVoltage = analogRead(PIN_PTrR) / (float)4096 * 3.3 ;
 //gfBlueAveragedVoltage = fBCurrentVoltage  * 0.1 + gfBlueAveragedVoltage * (1-0.1); 
 //gfYellowAveragedVoltage = fYCurrentVoltage  * 0.1 + gfYellowAveragedVoltage * (1-0.1); 
 //gfRedAveragedVoltage = fRCurrentVoltage  * 0.1 + gfRedAveragedVoltage * (1-0.1); 
 gfBlueAveragedVoltage = fBCurrentVoltage;
 gfYellowAveragedVoltage = fYCurrentVoltage;
 gfRedAveragedVoltage = fRCurrentVoltage;
 
 //gu32Count++;
 //}
}
void setup() {
 Serial.begin(115200);
 Serial.println("DHT11");
 pinMode(PIN_LED, OUTPUT);
 dht.begin();
 gviCountWatchDog=0; 
 init_TimerInterrupt2();
 initWifiClient();
 connectMqtt();
}
void loop() {
 portENTER_CRITICAL(&timerMux2);
 gviCountWatchDog=0; //clear Counter
 portEXIT_CRITICAL(&timerMux2);
 bool isFahrenheit = false;
 float percentHumidity = dht.readHumidity();
 float temperature = dht.readTemperature( isFahrenheit );
 if (isnan(percentHumidity) || isnan(temperature)) {
 Serial.println("ERROR");
 return;
 }
 if ( WiFi.status() != WL_CONNECTED ) {
 initWifiClient();
 }
 //パトライト各色初期値(平均化処理前段)
 gfBlueAveragedVoltage = 1.0;
 gfYellowAveragedVoltage = 1.0;
 gfRedAveragedVoltage = 1.0;
 
 //パトライトスキャン青B,黄Y,赤R  平均化処理平均化処理時　1秒ディレイ
 LampScan();
 //平均化カウンタリセット
 gu32Count = 0;
 //追加ディレイインターバル　1000 = 1秒 //追加ディレイインターバル　1000 = 1秒
 delay(500);
 
 // heatIndex : 体感温度
 float heatIndex = dht.computeHeatIndex(
 temperature, 
 percentHumidity, 
 isFahrenheit);
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
 //cValue = (char *)malloc(14);
 //sprintf(cValue, "%3.1f , %3.1f", temperature,percentHumidity);
 cValue = (char *)malloc(json.length()+1);
 json.toCharArray(cValue, json.length()+1 );
 
 doPubMqtt(cValue);
 free(cValue);
 //Serial.println(s);
 portENTER_CRITICAL(&timerMux2);
 gviCountWatchDog=0; //clear Counter
 portEXIT_CRITICAL(&timerMux2);
 delay(1500);
}
```

