//node mcu all notes

esp8266 wifi chip

9 digital pins
d0-d8
tx/rx pins 
power supply 3.3v&GND
1-A0
s0 to s3 for external peripherals like ram , memory,connection to another microprocceres
input voltage 7V-12V

-----------------------------------
how to connect wifi to node mcu
#include<ESP8266WiFi.h>
WiFi.begin("ssid","password");  //command to connect to wifi network

WiFi.status();

values: 1)WL_CONNECTED
        2)WL_IDLE_STATUS
        WL_CONNECT_FAILED

//CODE

#include<ESP8266WiFi.h>

void setup()
{
Serial.begin(9600);
WiFi.begin("ssid","password");

while(WiFi.status()!=WL_CONNECTED)
{
Serial.print("..");
delay(200);
}
Serial.println("node mcu connected");
//getting ip of wifi
Serial.println(WiFi.localIP());
}

-----------------------
client server model using node

#include<ESP8266WiFi.h>
#define led D5
WiFiClient client;
WifiServer server(80);

void setup()
{
Serial.begin(9600);
WiFi.begin("ssid","password");

while(WiFi.status()!=WL_CONNECTED)
{
Serial.print("..");
delay(200);
}
Serial.println("node mcu connected");
//getting ip of wifi
Serial.println(WiFi.localIP());
pinMode(led,OUTPUT);
server.begin();
}

void loop()
{
client=server.available(); //gets a client that is connetced to the server and has data available for reading

if(client==1)
{
String request==client.readStringUntil('\n');
Serial.println(request);
request.trim();
if(request=="GET/ledon HTTP/1.1")
{
digitalWrite(led,HIGH);
}
else
{
digitalWrite(led,LOW); 
}
}

}


-----------------------
creating node mcu a wifi access point
#include<ESP8266WiFi.h>

WiFiClient client;
WiFiServer server(80);

void setup()
{
Serial.begin(9600);
WiFi.softAP("NodeMCU","123456789"); //node mcu as a hotspot here AP means access point
Serial.println();
Serial.println("NodeMCU Started!");
Serial.println(WiFi.softAPIP());
server.begin();
}
void loop()
{
client=server.available(); //gets a client that is connetced to the server and has data available for reading

if(client==1)
{
String request==client.readStringUntil('\n');
Serial.println(request);
request.trim();
if(request=="GET/ledon HTTP/1.1")
{
digitalWrite(led,HIGH);
}
else
{
digitalWrite(led,LOW); 
}
}

}
