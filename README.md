CODE IN MPU6050:
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>
Adafruit_MPU6050 mpu;
void SensorSetup()
{
mpu.setAccelerometerRange(MPU6050_RANGE_8_G);
Serial.print("Accelerometer range set to: ");
switch (mpu.getAccelerometerRange()) {
case MPU6050_RANGE_2_G:
Serial.println("+-2G");
break;
case MPU6050_RANGE_4_G:
Serial.println("+-4G");
break;
case MPU6050_RANGE_8_G:
Serial.println("+-8G");
break;
case MPU6050_RANGE_16_G:
Serial.println("+-16G");
break;
}
mpu.setGyroRange(MPU6050_RANGE_500_DEG);
Serial.print("Gyro range set to: ");
switch (mpu.getGyroRange()) {
case MPU6050_RANGE_250_DEG:
Serial.println("+- 250 deg/s");
break;
case MPU6050_RANGE_500_DEG:
Serial.println("+- 500 deg/s");
break;
case MPU6050_RANGE_1000_DEG:
Serial.println("+- 1000 deg/s");
break;
case MPU6050_RANGE_2000_DEG:
Serial.println("+- 2000 deg/s");
break;
}
mpu.setFilterBandwidth(MPU6050_BAND_21_HZ);
Serial.print("Filter bandwidth set to: ");
switch (mpu.getFilterBandwidth()) {
case MPU6050_BAND_260_HZ:
Serial.println("260 Hz");
break;
case MPU6050_BAND_184_HZ:
Serial.println("184 Hz");
break;
case MPU6050_BAND_94_HZ:
Serial.println("94 Hz");
break;
case MPU6050_BAND_44_HZ:
Serial.println("44 Hz");
break;
case MPU6050_BAND_21_HZ:
Serial.println("21 Hz");
break;
case MPU6050_BAND_10_HZ:
Serial.println("10 Hz");
break;
case MPU6050_BAND_5_HZ:
Serial.println("5 Hz");
break;
}
}
void MPU6050_Init()
{
if (!mpu.begin()) {
lcd.clear();
lcd.print("MPU Not Found");
while (1) {
delay(10);
}
}
lcd.clear();
lcd.print("MPU6050 Found!");
delay(1000);
SensorSetup();
}
CODE IN GSMGPS:
#include <Wire.h>
#include<LiquidCrystal_I2C.h>
#include <SoftwareSerial.h>
#include <TinyGPS.h>
LiquidCrystal_I2C lcd(0x27,16,2);
SoftwareSerial gsm(3, 2);
TinyGPS gps;
char number1[13];
char number2[13];
char number3[13];
char rcv,rcv1,rcv2,rcv3;
int ii,i;
int NumberCount=0;
float lat, lon;
void gpsread()
{
for (unsigned long start = millis(); millis() - start < 1000;)
{
while(Serial.available())
{
if(gps.encode(Serial.read()))// encode gps data
{
lcd.clear();
lcd.setCursor(0,0);
lcd.print("LAT:");
cd.setCursor(4,0);
lcd.print(lat,6);
lcd.setCursor(0,1);
lcd.print("LON:");
lcd.setCursor(4,1);
lcd.print(lon,6);
delay(1000);
}
}
}
}
void SerialInit()
{
Serial.begin(9600);
gsm.begin(9600);
}
void LCDInit()
{
lcd.begin();
lcd.backlight();
}
void ok()
{
do{rcv = gsm.read();}while(rcv != 'K');
}
void GSMInit()
{
gsm.println("AT");ok();
gsm.println("AT+CLIP=1");ok();
gsm.println("AT+CMGF=1");ok();
gsm.println("AT+CNMI=1,2,0,0");ok();
//gsm.println("AT+CIPSHUT");ok();
//gsm.println("AT+CIPMUX=0");ok();
//gsm.println("AT+CGATT=1");ok();
//gsm.println("AT+CSTT=\"APN\",\"USER NAME\",\"PASSWORD\"");ok();
//gsm.println("AT+CIICR");ok();
//gsm.println("AT+CIFSR");
//delay(3000);
}
{
void storenumber1()
do
{
do
{
while ( !gsm.available() );
} while ( '+' != gsm.read() );
while(!gsm.available());
} while ( 'C' != gsm.read() );
do
{
while ( !gsm.available() );
} while ( ':' != gsm.read() );
do
{
while ( !gsm.available() );
} while ( '"' != gsm.read() );
ii = 0;
do
{
while ( !gsm.available() );
while ( !gsm.available() );
rcv1 = gsm.read();
number1 [ ii ] = rcv1;
ii ++;
} while ( ii!= 13 );
lcd.clear();
i=0;
do{
lcd.write(number1[i]);
i++;
}while(i!=13);
delay(1000);
gsm.println("ATH");
delay(1000);
}
void storenumber2()
{
do
{
do
{
while ( !gsm.available() );
} while ( '+' != gsm.read() );
while(!gsm.available());
} while ( 'C' != gsm.read() );
do
{
while ( !gsm.available() );
} while ( ':' != gsm.read() );
do
{
while ( !gsm.available() );
} while ( '"' != gsm.read() );
ii = 0;
do
{
while ( !gsm.available() );
while ( !gsm.available() );
rcv2 = gsm.read();
number2 [ ii ] = rcv2;
ii ++;
} while ( ii!= 13 );
lcd.clear();
i=0;
do{
lcd.write(number2[i]);
i++;
}while(i!=13);
delay(1000);
gsm.println("ATH");
delay(1000);
}
void storenumber3()
{
do
{
do
{
while ( !gsm.available() );
} while ( '+' != gsm.read() );
while(!gsm.available());
} while ( 'C' != gsm.read() );
do
{
while ( !gsm.available() );
} while ( ':' != gsm.read() );
do
{
while ( !gsm.available() );
} while ( '"' != gsm.read() );
ii = 0;
do
{
while ( !gsm.available() );
while ( !gsm.available() );
rcv3 = gsm.read();
number3 [ ii ] = rcv3;
ii ++;
} while ( ii!= 13 );
lcd.clear();
i=0;
do{
lcd.write(number3[i]);
i++;
}while(i!=13);
delay(1000);
gsm.println("ATH");
delay(1000);
}
void sendregsms1()
{
/*gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number1[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.print("YOUR MOBILE NUMBER REGISTERED");
gsm.write(26);
ok();*/
lcd.clear();
lcd.print("Your Mobile NUM");
lcd.setCursor(0,1);
lcd.print("Registered");
delay(1000);
}
void sendregsms2()
{
/*gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number2[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.print("YOUR MOBILE NUMBER REGISTERED");
gsm.write(26);
ok();*/
lcd.clear();
lcd.print("Your Mobile NUM");
lcd.setCursor(0,1);
lcd.print("Registered");
delay(1000);
}
void sendregsms3()
{
/*gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number3[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.print("YOUR MOBILE NUMBER REGISTERED");
gsm.write(26);
ok();*/
lcd.clear();
lcd.print("Your Mobile NUM");
lcd.setCursor(0,1);
lcd.print("Registered");
delay(1000);
}
{
void Number_1()
lcd.clear();
lcd.print("GIVE MIS CALL TO");
lcd.setCursor(0,1);
lcd.print("STORE NUMBER1");
storenumber1();
delay(1000);
lcd.clear();
lcd.print("SMS SENDING");
sendregsms1();
delay(100);
lcd.clear();
lcd.print("SMS SENT");
}
void Number_2()
{
lcd.clear();
lcd.print("GIVE MIS CALL TO");
lcd.setCursor(0,1);
lcd.print("STORE NUMBER2");
storenumber2();
delay(1000);
lcd.clear();
lcd.print("SMS SENDING");
sendregsms2();
delay(100);
lcd.clear();
lcd.print("SMS SENT");
}
void Number_3()
{
lcd.clear();
lcd.print("GIVE MIS CALL TO");
lcd.setCursor(0,1);
lcd.print("STORE NUMBER3");
storenumber3();
delay(1000);
lcd.clear();
lcd.print("SMS SENDING");
sendregsms3();
delay(100);
lcd.clear();
lcd.print("SMS SENT");
}
void StoreMobileNumbers()
{
lcd.clear();
lcd.print("GSM INITILIZING");
delay(1000);
GSMInit();
lcd.clear();
lcd.print("GSM INIT COMPLE");
delay(1000);
Number_1();
Number_2();
Number_3();
}
void GPSInit()
{
lcd.clear();
lcd.print("GPS Initilizing");
delay(1000);
gpsread();
lcd.clear();
lcd.print("GPS Init Complete");
delay(1000);
}
void ShowSerialData()
{
while(gsm.available()!=0)
Serial.write(gsm.read());
delay(5000);
}
void Accident1()
{
gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number1[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.println("ACCIDENT DETECTED");
gsm.print("VEHICLE LOCATION");
gsm.println();
gsm.print("https://www.google.co.in/maps/search/");
gsm.print(lat,6);
gsm.write(',');
gsm.print(lon,6);
gsm.print("\r\n");
gsm.write(26);
ok();
}
void Accident2()
{
gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number2[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.println("ACCIDENT DETECTED");
gsm.print("VEHICLE LOCATION");
gsm.println();
gsm.print("https://www.google.co.in/maps/search/");
gsm.print(lat,6);
gsm.write(',');
gsm.print(lon,6);
gsm.print("\r\n");
gsm.write(26);
ok();
}
void Accident3()
{
gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number3[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.println("ACCIDENT DETECTED");
gsm.print("VEHICLE LOCATION");
gsm.println();
gsm.print("https://www.google.co.in/maps/search/");
gsm.print(lat,6);
gsm.write(',');
gsm.print(lon,6);
gsm.print("\r\n");
gsm.write(26);
ok();
}
void help1()
{
gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number1[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.print("VEHICLE LOCATION");
gsm.println();
gsm.print("https://www.google.co.in/maps/search/");
gsm.print(lat,6);
gsm.write(',');
gsm.print(lon,6);
gsm.print("\r\n");
gsm.write(26);
ok();
}
void help2()
{
gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number2[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.print("VEHICLE LOCATION");
gsm.println();
gsm.print("https://www.google.co.in/maps/search/");
gsm.print(lat,6);
gsm.write(',');
gsm.print(lon,6);
gsm.print("\r\n");
gsm.write(26);
ok();
}
void help3()
{
gsm.print("AT+CMGS=");
gsm.write('"');
i=0;
do{
gsm.write(number3[i]);
i++;
}while(i!=13);
gsm.write('"');
gsm.print("\r\n");
do{
rcv = gsm.read();
}while(rcv != '>');
gsm.print("VEHICLE LOCATION");
gsm.println();
gsm.print("https://www.google.co.in/maps/search/");
gsm.print(lat,6);
gsm.write(',');
gsm.print(lon,6);
gsm.print("\r\n");
gsm.write(26);
ok();
}
CODE IN IOT:
#include "GSMGPS.h"
#include "MPU6050_Sensor.h"
//#include "ThingSpeakServer.h"
//long writingTimer = 20;
//long startTime = 0;
//long waitTime = 0;
/*void writeThingSpeak()
{
gsm.println("AT+CIPSTART=\"TCP\",\"api.thingspeak.com\",\"80\"");//s
tart up the connection
delay(1000);
ShowSerialData();
gsm.println("AT+CIPSEND");//begin send data to remote server
delay(1000);
ShowSerialData();
String str="GET
https://api.thingspeak.com/update?api_key=XYAQE37CLRVDKP6I&field1="
+
String(random(0,100)) +"&field2="+
String(random(100,200));/* +"&field3="+
String(hh) +"&field4="+
String(tt)+"&field5="+
String(WaterTemperature);//+"&field6="+*/
/*// String(turbidity);
Serial.println(str);
gsm.println(str);//begin send data to remote server
delay(1000);
ShowSerialData();
gsm.println((char)26);//sending
delay(1000);//waitting for reply, important! the time is base on
the condition of internet
gsm.println();
ShowSerialData();
}*/
void setup()
{
SerialInit();
LCDInit();
StoreMobileNumbers();
GPSInit();
MPU6050_Init();
lcd.clear();
lcd.print("BIKE ACCIDENT");
lcd.setCursor(0,1);
lcd.print("MONITORING SYS");
delay(2000);
lp:
lcd.clear();
lcd.print("SEND SMS TO TRACK");
lcd.setCursor(0,1);
lcd.print("VEHICLE POSITION");
while(1)
{
do{
rcv=gsm.read();
sensors_event_t a, g, temp;
mpu.getEvent(&a, &g, &temp);
if(a.acceleration.y>6.0)
{
delay(5000);
if(a.acceleration.y>6.0)
{
lcd.clear();
lcd.print("Accident Detected");
delay(1000);
lcd.clear();
lcd.print("Sending SMS");
Accident1();
Accident2();
Accident3();
lcd.clear();
lcd.print("SMS Sent");
goto lp;
{
}
}
}while(rcv!='*');
if(rcv=='*')
lcd.clear();
lcd.print("COMAND RECEIVE");
delay(1000);
lcd.clear();
lcd.print("VEHICLE LOCATION");
delay(1000);
lcd.clear();
lcd.setCursor(0,0);
lcd.print("LAT:");
lcd.setCursor(4,0);
lcd.print(lat,6);
lcd.setCursor(0,1);
lcd.print("LON:");
lcd.setCursor(4,1);
lcd.print(lon,6);
delay(2000);
lcd.clear();
lcd.print("SENDING SMS");
help1();
help2();
help3();
delay(1000);
lcd.clear();
lcd.print("SMS SENT");
delay(1000);
goto lp;
}
}
}
void loop()
{
;;;
}
