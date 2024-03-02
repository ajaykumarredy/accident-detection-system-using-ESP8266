#include<ESP8266WiFi.h> 
#include<PubSubClient.h>
constchar*ssid="YOUR_WIFI_SSID";
constchar*password="YOUR_WIFI_PASSWORD"; 
const char* mqtt_server = "MQTT_BROKER_IP"; 
const char* topic = "accident/alert";
WiFiClient espClient; 
PubSubClientclient(espClient);
voidsetup_wifi(){ 
delay(10); 
Serial.println();
Serial.print("Connectingto"); 
Serial.println(ssid);
WiFi.begin(ssid,password);
while(WiFi.status()!=WL_CONNECTED){ delay(500);
Serial.print(".");
}
Serial.println(""); 
Serial.println("WiFiconnected"); 
Serial.println("IP address: "); 
Serial.println(WiFi.localIP());
}
voidcallback(char*topic,byte*payload,unsignedintlength){ Serial.print("Message 
arrived [");
Serial.print(topic); 
Serial.print("] ");
for (int i = 0; i < length; i++) { 
Serial.print((char)payload[i]);
}
Serial.println();
}
voidreconnect(){
while (!client.connected()) { 
Serial.print("AttemptingMQTTconnection..."); 
if (client.connect("ESP8266Client")) { 
Serial.println("connected"); 
client.subscribe(topic);
} else 
{Serial.print("failed,rc=")
;
Serial.print(client.state()); 
Serial.println("tryagainin5seconds"); 
delay(5000);
}}}
void setup() { 
Serial.begin(115200); 
setup_wifi();
client.setServer(mqtt_server, 1883); 
client.setCallback(callback);
}
voidloop(){
if(!client.connected()){ 
reconnect();
}
client.loop();
//Codetodetectaccidentsusingsensorsandtriggeralerts
}
