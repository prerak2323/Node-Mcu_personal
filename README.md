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

{
}
}

}


-------------------------------------------------------

//accessing node mcu using webpage

html code

<a herf="ip_add/ledon"><button>LED ON</button></a>
<a herf="ip_add.ledoff"><button>LED OFF</button></a>

just add the above code and make sure to use herf and put the correct ip add and link

change
void loop()
{
client.println("HTTP/1.1 200 OK"); 
client.println("Content-Type: text/html");
client.println("");
client.println("<!DOCTYPE HTML>");
client.println("<html>");
client.println("<h1>welcome to webpage");
client.println("<h3>LED Controls");
client.println("<br>");
client.println("<a herf=''><button>led on</buttion>");
client.println("<a herf=''><button>led off</button>");
client.println("</html>");

}
-----------------------------------
sending data of node mcu to think speed cloud
library-1.thinkspeak
#include<ESP8266WiFi.h>
#include<DHT.h>
#include<ThinkSpeak.h>

DHT dht(D5,DHT11);

WiFiClient client;

long myChannelNumber=1047069;  //Channel Id
const char myWriteAPIKey[]="Q755JUK5LXWNFQSN";

void setup()
{
Serial.begin(9600);
WiFi.begin('ssid','password');
while(WiFi.status()!=WL_CONNECTED)
{
delay(200);
Serial.print("..");
}
Serial.println();
Serial.println('nodemcu is connected');
Serial.println(WiFi.localIP());
dhr.begin();
ThingSpeak.begin(client);
}
void loop()
{
float h=dht.readHumidity();
float t=dht.readTemperature();
Serial.println('temperature: '+(string) t);
Serial.println('Humidity: '+(string) h);
ThingSpeak.writeField(myChannelNumber,1,t,myWriteAPIKey);
delay(2000);
}
