# ex1 การเขียนโปรแกรมเพื่อรันบนไมโครคอนโทรเลอร์
 #include <Arduino.h>

 int cnt = 0;
 

 void setup()
 
 
 {
	Serial.begin(115200);
	
 }
 

 void loop()
 
 {
 
 	cnt++;
	
	Serial.printf("A:%d\n",cnt);
	
	delay(300);
	
 }
 
# ex2 การเขียนโปรแกรมค้นหาไวไฟ
 #include <Arduino.h>
 #include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	WiFi.mode(WIFI_STA);
	WiFi.disconnect();
	delay(100);
	Serial.println("\n\n\n");
}

void loop()
{
	Serial.println("========== เริ่มต้นแสกนหา Wifi ===========");
	int n = WiFi.scanNetworks();
	if(n == 0) {
		Serial.println("NO NETWORK FOUND");
	} else {
		for(int i=0; i<n; i++) {
			Serial.print(i + 1);
			Serial.print(": ");
			Serial.print(WiFi.SSID(i));
			Serial.print(" (");
			Serial.print(WiFi.RSSI(i));
			Serial.print(")");
			Serial.println((WiFi.encryptionType(i) == ENC_TYPE_NONE)?" ":"*");
			delay(10);
		}
	}
	Serial.println("\n\n");
}
# ex3 การเขียนโปรแกรมเอ้าพุทสัญญาณดิจิทัล
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	pinMode(0, OUTPUT);
	Serial.println("\n\n\n");
}

void loop()
{
	cnt++;
	if(cnt % 2) {
		Serial.println("========== ON ===========");
		digitalWrite(0, HIGH);
	} else {
		Serial.println("========== OFF ===========");
		digitalWrite(0, LOW);
	}
	delay(500);
}
# ex4 การเขียนโปรแกรมอินพุทสัญญาณดิจิทัล
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "HI_BMFWIFI_2.4G";
const char* password = "0819110933";

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.mode(WIFI_STA);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.print("\n\nIP address: ");
	Serial.println(WiFi.localIP());

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}
# ex5 การเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์
#include <ESP8266WiFi.h>
#include <ESP8266WiFiMulti.h>
#include <ESP8266WebServer.h>

const char* ssid = "HI_BMFWIFI_2.4G";
const char* password = "0819110933";

ESP8266WiFiMulti wifiMulti;
ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	wifiMulti.addAP(ssid, password);

	Serial.println("Connecting ...");
	int i = 0;
	while (wifiMulti.run() != WL_CONNECTED) { 
		delay(1000);
		Serial.print(++i); Serial.print(' ');
	}
	Serial.println("");
	Serial.print("IP address: ");
	Serial.println(WiFi.localIP());

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}
# ex6 การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	pinMode(0, OUTPUT);
	pinMode(2, OUTPUT);
	Serial.println("\n\n\n");
}

void loop()
{
	cnt++;
	if(cnt % 2) {
		Serial.println("========== ON 0,2 ===========");
		digitalWrite(0, HIGH);
		digitalWrite(2, HIGH);
	} else {
		Serial.println("========== OFF 0,2 ===========");
		digitalWrite(0, LOW);
		digitalWrite(2, LOW);
	}
	delay(500);
}
