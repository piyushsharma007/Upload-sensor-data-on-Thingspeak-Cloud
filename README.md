#include <SoftwareSerial.h>
// replace with your channel's thingspeak API key
String apiKey = "1MWARBDKE JSDNGD";
SoftwareSerial ser(2, 3); // RX, TX
// this runs once
void setup() { 
 // enable debug serial
 Serial.begin(115200); 
 // enable software serial
 ser.begin(115200);
}
// the loop 
void loop() {
 int value=analogRead(A0);
 String state1=String(value);
 // TCP connection
 String cmd = "AT+CIPSTART=\"TCP\",\"";
 cmd += "184.106.153.149"; // api.thingspeak.com
 cmd += "\",80";
 ser.println(cmd);
 Serial.println(cmd);
 if(ser.find("Error")){
 Serial.println("AT+CIPSTART error");
 return;
 }
 // prepare GET string
 String getStr = "GET /update?api_key=";
 getStr += apiKey;
getStr +="&field1=";
 getStr += String(state1);
// getStr +="&field2=";
 //getStr += String(state2);
 getStr += "\r\n\r\n";
 // send data length
 cmd = "AT+CIPSEND=";
 cmd += String(getStr.length());
 ser.println(cmd);
 Serial.println(cmd);
 if(ser.find(">")){
 ser.print(getStr);
 Serial.print(getStr);
 }
 else{
 ser.println("AT+CIPCLOSE");
 // alert user
 Serial.println("AT+CIPCLOSE");
 }
 // thingspeak needs 15 sec delay between updates
 delay(15000); 
}
