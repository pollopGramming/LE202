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
 
 - กำหนดค่า cnt
 - มี setup
 - มี วนลูป
 - มี ดีเล พื่อให้โค้ตไม่รันถี่เกินไป
 
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
 - กำหนดค่า cnt
 - มี setup
 - มี วนลูป
 - มี ดีเล พื่อให้โค้ตไม่รันถี่เกินไป
 - เมื่อเริ่มวนรูปจะขึ้นข้อความ ========== เริ่มต้นแสกนหา Wifi ===========
 - ถ้าไม่เจอสัญญาณะขึ้น NO NETWORK FOUND
 - ถ้าเจอสัญญาณจะขึ้น ชื่อสัญญาณ
# ex3 เรื่อง การเขียนโปรแกรมเอ้าพุทสัญญาณดิจิทัล

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
 - กำหนดค่า cnt
 - มี setup
 - มี วนลูป
 - มี ดีเล พื่อให้โค้ตไม่รันถี่เกินไป
 - สร้างให้ cnt บวก 1 ไปเลื่อยๆ
 - เมื่อ cnt %2 แล้ว
 - ลงตัวจะเเสดงค่า on
 - ไม่ลงตัวจะเป็นค่า off


# ex4 การเขียนโปรแกรมอินพุทสัญญาณดิจิทัล

#include <Arduino.h>

#include <ESP8266WiFi.h>


int cnt = 0;


void setup()

{

	Serial.begin(115200);
	
	pinMode(0, INPUT);
	
	pinMode(2, OUTPUT);
	
	Serial.println("\n\n\n");
	
}


void loop()

{

	int val = digitalRead(0);
	
	Serial.printf("======= read %d\n", val);
	
	if(val==1) {
	
	
	
		digitalWrite(2, LOW);
		
	} else {
	
		digitalWrite(2, HIGH);
		
	}
	
	delay(100);
	
}


 - กำหนดค่า cnt
 - มี setup
 - มี วนลูป
 - มี ดีเล พื่อให้โค้ตไม่รันถี่เกินไป
 - ให้ port0 เป็น input
 - ให้ port2 เป็น output
 - รับค่าจะ port0
 - ถ้าเป็น 1 จะให้ low ส่งไป port2 ไปจะดับ
 - ถ้าเป็น 0 จะให้ high ส่งไป port2 ไปจะติด

# ex5 รื่อง การเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

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
 - กำหนดค่า cnt
 - มี setup
 - มี วนลูป
 - มี ดีเล พื่อให้โค้ตไม่รันถี่เกินไป
 - ให้ใส่ชื่อ และ พาสเวิด
 - ถ้าเชื่อสำเร็จ จะขึ้น hello
 - ถ้าไม่สำเร็จจะขึ้น Path Not Fonud

# ex6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

#include <ESP8266WiFi.h>

//#include <WiFiClient.h>

#include <ESP8266WebServer.h>



const char* ssid = "MY-ESP8266";

const char* password = "choompol";


IPAddress local_ip(192, 168, 1, 1);

IPAddress gateway(192, 168, 1, 1);

IPAddress subnet(255, 255, 255, 0);



ESP8266WebServer server(80);



int cnt = 0;

void setup(void){

	Serial.begin(115200);
	

	WiFi.softAP(ssid, password);
	
	WiFi.softAPConfig(local_ip, gateway, subnet);
	
	delay(100);

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


 - กำหนดค่า cnt
 - มี setup
 - มี วนลูป
 - มี ดีเล พื่อให้โค้ตไม่รันถี่เกินไป
 - ให้ใส่ชื่อ และ พาสเวิด
 - ตั้งค่า local_ip
 - ตั้งค่า gateway
 - ตั้งค่า subnet
 - ถ้ามีคนเชื่อมต่อสำเร็จจะขึ้น hello
 - ถ้าไม่ได้จะขึ้น Path Not Fonud

