# NAGARAJ
 
 #include <ESP8266WiFi.h>  
 #include <WiFiClient.h>  
 #include <ThingSpeak.h>  
 int offset =0;

#include "HX711.h"  
HX711 scale(D1, D2);
float calibration_factor = -402; 

 const char* ssid = "klab";  
 const char* password = "Madurai625016";  
 WiFiClient client;  
 unsigned long myChannelNumber = 459167;  
 const char * myWriteAPIKey = "6H8KQX65OSE2DWJM";  
 
 void setup()  
 {  
  Serial.begin(9600);  
  delay(100);  
   Serial.println("Press T to tare");
  scale.set_scale(-402);  
  scale.tare();   

  // Connect to WiFi network  
  Serial.println();  
  Serial.println();  
  Serial.print("Connecting to ");  
  Serial.println(ssid);  
  WiFi.begin(ssid, password);  
  while (WiFi.status() != WL_CONNECTED)  
  {  
   delay(500);  
   Serial.print(".");  
  }  
  Serial.println("");  
  Serial.println("WiFi connected");  
  // Print the IP address  
  Serial.println(WiFi.localIP());  
  ThingSpeak.begin(client);  
 }  
 void loop()   
 {  
    printVolts();
  Loadcell();


  // Write to ThingSpeak. There are up to 8 fields in a channel, allowing you to store up to 8 different  
  // pieces of information in a channel. Here, we write to field 1.  
 
  // ThingSpeak will only accept updates every 15 seconds.  
 }  
void printVolts()
{
 int volt = analogRead(A0);// read the input
  double vin = map(volt,0,1023, 0, 2500) + offset;// map 0-1023 to 0-2500 and add correction offset
  
  vin /=100;// divide by 100 to get the decimal values
  //Serial.print("Voltage: ");
  //Serial.print(vin);//print the voltge
  //Serial.println("V");
                                                                                                                                                                                                                                            Serial.println(" LAT :9.8360096");
                                                                                                                                                                                                                                            Serial.println("LON: 78.1570412");
delay(500);
 int B;
if(vin>9.46) {
   Serial.print("                     Battery level is 100%");
   B=100;
   Serial.println(B);}
else { Serial.print(" ");}
if(vin<=9.46 && vin>9.28) {
  Serial.print("              Battery level is 95%");
  B=95;}
else {Serial.print(" ");}
if(vin<=9.28 && vin>9.12) {Serial.print("               Battery level is 90%");
B=90;}
else { Serial.print(" ");}
if(vin<=9.12 && vin>8.98) {Serial.print("              Battery level is 80%");
B=80;}
else { Serial.print(" ");}
if(vin<=8.98 && vin>8.90){Serial.print("               Battery level is 70%");
B=70;}
else {Serial.print(" ");}
if(vin<=8.90) {Serial.print("                        Battery about to die");
B=10;}
else {Serial.print(" ");}
delay(200);
    ThingSpeak.writeField(myChannelNumber, 2,B, myWriteAPIKey); 
  }
void Loadcell(){Serial.print("             Weight: ");
  
  Serial.print(scale.get_units(), 2);  
int L=scale.get_units();
  Serial.print(L,2);
  Serial.print("grams"); 
   ThingSpeak.writeField(myChannelNumber, 1,L, myWriteAPIKey);
 
  if(Serial.available())
  {
    char temp = Serial.read();
    if(temp == 't' || temp == 'T')
      scale.tare();       
  }}
