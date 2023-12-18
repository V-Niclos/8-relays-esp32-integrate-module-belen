# 8-relays-esp32-integrate-module-belen
Integrated module esp32 with 8 relays, managed with alexa and web interface


![alt text](belen_alexa_8_reles_2023_12_14/documentation/relays_module.jpg)

Modulo esp32 con 8 reles integrado
Power 5v 2a
Conexion a Alexa
Alexa names of relays [1...8] Belen 1... Belen 8

#First start 
By default or conection not valid the module will be started as access point with name NEWIOT-XXX pwd 123456789 and ip 192.168.4.1
Yo need conecc at access point, (newiot-xxx, pwd=123456789) and open your browser url 192.168.4.1 then change configuration for connect to you wifi 

# the module use in my version
```c++
#ifndef MAINDEFINES_H
#define MAINDEFINES_H
#include <Arduino.h>
#include "AsyncJson.h"
#include "ArduinoJson.h"
#include <WiFi.h>
#include <ESPAsyncWebServer.h>
#include "fauxmoESP.h"
#include "ClsRelay.h"
#include "ClsTimeSun.h"
#include "ClsWifiConfig.h"
#define SERIAL_BAUDRATE  115200
  StaticJsonDocument<1024> g_docJsonRelays;
ClsTimeSun g_TimeSun;
ClsWifiConfig g_Config;
//------ RELAYS
uint32_t g_RelayDelayLoop = 1;
const int g_RelaysCount = 8;
ClsRelay g_Relays[g_RelaysCount];
//------ HADWARE PINS USED
const int g_pinIntLedBlue =2;
const int g_pingSensorLed = 23;

const int g_pinRelays[g_RelaysCount] = {13,12,14,27,26,25,33,32};

//-------------- NET SERVICES 
unsigned long g_AlexaLastMillis =0;
fauxmoESP g_NetFauxmoAlexa;
AsyncWebServer g_NetWebServer(80);
// THIS IS THE NAMES joined to reralys TO SAY alexa command 
// example like: "Alexa! turn on relay 1"
const char g_AlexaDeviceNames[8][8]={{"belen 1"},{"belen 2"},{"belen 3"},{"belen 4"},{"belen 5"},{"belen 6"},{"belen 7"},{"belen 8"}};

TaskHandle_t g_MainTaskAlexaHandle;
TaskHandle_t g_MainTaskTimeSunHandle;
TaskHandle_t g_MainTaskRelaysHandle;
void g_MainTaskAlexa(void *pvParameters);
void g_MainTaskTimeSun(void *pvParameters);
void g_MainTaskRelays(void *pvParameters);

bool fncRelaysJsonApply(String sJson);

#endif
```

const int g_pinRelays[g_RelaysCount] = {13,12,14,27,26,25,33,32};

