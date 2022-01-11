
Could someone help me how to modify this library and be able to use more dmx universes please  

\#include <FastLED.h>  // include FastLED \*before\* Artnet
  

  
// Please include ArtnetEther.h to use Artnet on the platform
  
// which can use both WiFi and Ethernet
  
\#include <ArtnetEther.h>
  
// this is also valid for other platforms which can use only Ethernet
  
// #include <Artnet.h>
  

  
// Ethernet stuff
  
const IPAddress ip(192, 168, 0, 50);
  
uint8\_t mac\[\] = {0x01, 0x23, 0x45, 0x67, 0x89, 0xAB};
  

  
ArtnetReceiver artnet;
  
uint8\_t universe = 0;  // 0 - 15
  

  
// FastLED
  
\#define NUM\_LEDS 600
  
CRGB leds\[NUM\_LEDS\];
  
const uint8\_t PIN\_LED\_DATA = 6;
  

  
void setup() {
  
Serial.begin(115200);
  
delay(2000);
  

  
FastLED.addLeds<NEOPIXEL, PIN\_LED\_DATA>(leds, NUM\_LEDS);
  

  
Ethernet.begin(mac, ip);
  
artnet.begin();
  
// artnet.subscribe\_net(0);     // optionally you can change
  
// artnet.subscribe\_subnet(0);  // optionally you can change
  

  
// if Artnet packet comes to this universe, forward them to fastled directly
  
artnet.forward(universe, leds, NUM\_LEDS);
  

  
// this can be achieved manually as follows
  
// if Artnet packet comes to this universe, this function is called
  
// artnet.subscribe(universe, \[&\](uint8\_t\* data, uint16\_t size)
  
// {
  
//     // set led
  
//     // artnet data size per packet is 512 max
  
//     // so there is max 170 pixel per packet (per universe)
  
//     for (size\_t pixel = 0; pixel < NUM\_LEDS; ++pixel) {
  
//         size\_t idx = pixel \* 3;
  
//         leds\[pixel\].r = data\[idx + 0\];
  
//         leds\[pixel\].g = data\[idx + 1\];
  
//         leds\[pixel\].b = data\[idx + 2\];
  
//     }
  
// });
  
}
  

  
void loop() {
  
artnet.parse();  // check if artnet packet has come and execute callback
  
FastLED.show();

