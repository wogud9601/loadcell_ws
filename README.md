# loadcell_ws

#include <HX711_ADC.h>
#if defined(ESP8266)|| defined(ESP32) || defined(AVR)
#include <EEPROM.h>
#endif

//pin 번호 할당
const int HX711_dout1 = PB12; // 노란색
const int HX711_sck1 = PB13;  // 파란색

const int HX711_dout2 = PB14; 
const int HX711_sck2 = PB15; 

const int HX711_dout3 = PC15;
const int HX711_sck3 = PC14; 

const int HX711_dout4 = PB6; 
const int HX711_sck4 = PB5 ; 

const int HX711_dout5 = PB8; 
const int HX711_sck5 = PB7;  

const int HX711_dout6 = PA2;
const int HX711_sck6 = PA1; 

const int HX711_dout7 = PA4; 
const int HX711_sck7 = PA3; 

const int HX711_dout8 = PB11 ; 
const int HX711_sck8 = PB10; 

const int HX711_dout9 = PB1; 
const int HX711_sck9 = PB0; 

const int HX711_dout10 = PA7; 
const int HX711_sck10 = PA6;  

//HX711 constructor (dout pin, sck pin)
HX711_ADC LoadCell_1(HX711_dout1, HX711_sck1);
HX711_ADC LoadCell_2(HX711_dout2, HX711_sck2);
HX711_ADC LoadCell_3(HX711_dout3, HX711_sck3);
HX711_ADC LoadCell_4(HX711_dout4, HX711_sck4);
HX711_ADC LoadCell_5(HX711_dout5, HX711_sck5);

HX711_ADC LoadCell_6(HX711_dout6, HX711_sck6);
HX711_ADC LoadCell_7(HX711_dout7, HX711_sck7);
HX711_ADC LoadCell_8(HX711_dout8, HX711_sck8);
HX711_ADC LoadCell_9(HX711_dout9, HX711_sck9);
HX711_ADC LoadCell_10(HX711_dout10, HX711_sck10);



// eeprom address 할당
const int calVal_eepromAdress_1 = 0; 
const int calVal_eepromAdress_2 = 4; 
const int calVal_eepromAdress_3 = 8; 
const int calVal_eepromAdress_4 = 12; 
const int calVal_eepromAdress_5 = 16;

const int calVal_eepromAdress_6 = 20; 
const int calVal_eepromAdress_7 = 24; 
const int calVal_eepromAdress_8 = 28; 
const int calVal_eepromAdress_9 = 32; 
const int calVal_eepromAdress_10 = 36; 

unsigned long t = 0;

void setup() {
  Serial.begin(115200); delay(10);
  Serial.println();
  Serial.println("Starting...");

// calibration factor 할당
  float calibrationValue_1; 
  float calibrationValue_2; 
  float calibrationValue_3; 
  float calibrationValue_4; 
  float calibrationValue_5; 

  float calibrationValue_6; 
  float calibrationValue_7; 
  float calibrationValue_8; 
  float calibrationValue_9; 
  float calibrationValue_10; 

// calibaration factor 기입
  calibrationValue_1 = 1000; 
  calibrationValue_2 = 1000;
  calibrationValue_3 = 1000; 
  calibrationValue_4 = 1000; 
  calibrationValue_5 = 1000; 

  calibrationValue_6 = 1000; 
  calibrationValue_7 = 1000;
  calibrationValue_8 = 1000; 
  calibrationValue_9 = 1000; 
  calibrationValue_10 = 1000; 

#if defined(ESP8266) || defined(ESP32)
  //EEPROM.begin(512); // uncomment this if you use ESP8266 and want to fetch the value from eeprom
#endif
  //EEPROM.get(calVal_eepromAdress_1, calibrationValue_1); // uncomment this if you want to fetch the value from eeprom
  //EEPROM.get(calVal_eepromAdress_2, calibrationValue_2); // uncomment this if you want to fetch the value from eeprom

  LoadCell_1.begin();
  LoadCell_2.begin();
  LoadCell_3.begin();
  LoadCell_4.begin();
  LoadCell_5.begin();
  
  LoadCell_6.begin();
  LoadCell_7.begin();
  LoadCell_8.begin();
  LoadCell_9.begin();
  LoadCell_10.begin();

  unsigned long stabilizingtime = 2000; // tare preciscion can be improved by adding a few seconds of stabilizing time
  boolean _tare = true; //set this to false if you don't want tare to be performed in the next step
  byte loadcell_1_rdy = 0;
  byte loadcell_2_rdy = 0;
  byte loadcell_3_rdy = 0;
  byte loadcell_4_rdy = 0;
  byte loadcell_5_rdy = 0;

  byte loadcell_6_rdy = 0;
  byte loadcell_7_rdy = 0;
  byte loadcell_8_rdy = 0;
  byte loadcell_9_rdy = 0;
  byte loadcell_10_rdy =0;

  while ((loadcell_1_rdy + loadcell_2_rdy + loadcell_3_rdy + loadcell_4_rdy + loadcell_5_rdy + loadcell_6_rdy + loadcell_7_rdy + loadcell_8_rdy + loadcell_9_rdy + loadcell_10_rdy) < 10) { //run startup, stabilization and tare, both modules simultaniously
    if (!loadcell_1_rdy) loadcell_1_rdy = LoadCell_1.startMultiple(stabilizingtime, _tare);
    if (!loadcell_2_rdy) loadcell_2_rdy = LoadCell_2.startMultiple(stabilizingtime, _tare);
    if (!loadcell_3_rdy) loadcell_3_rdy = LoadCell_3.startMultiple(stabilizingtime, _tare);
    if (!loadcell_4_rdy) loadcell_4_rdy = LoadCell_4.startMultiple(stabilizingtime, _tare);
    if (!loadcell_5_rdy) loadcell_5_rdy = LoadCell_5.startMultiple(stabilizingtime, _tare);

    if (!loadcell_6_rdy) loadcell_6_rdy = LoadCell_6.startMultiple(stabilizingtime, _tare);
    if (!loadcell_7_rdy) loadcell_7_rdy = LoadCell_7.startMultiple(stabilizingtime, _tare);
    if (!loadcell_8_rdy) loadcell_8_rdy = LoadCell_8.startMultiple(stabilizingtime, _tare);
    if (!loadcell_9_rdy) loadcell_9_rdy = LoadCell_9.startMultiple(stabilizingtime, _tare);
    if (!loadcell_10_rdy) loadcell_10_rdy = LoadCell_10.startMultiple(stabilizingtime, _tare);
  }
  if (LoadCell_1.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.1 wiring and pin designations");
  }
  if (LoadCell_2.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.2 wiring and pin designations");
  }
  if (LoadCell_3.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.3 wiring and pin designations");
  }
  if (LoadCell_4.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.4 wiring and pin designations");
  }
  if (LoadCell_5.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.5 wiring and pin designations");
  }
  if (LoadCell_6.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.6 wiring and pin designations");
  }
  if (LoadCell_7.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.7 wiring and pin designations");
  }
  if (LoadCell_8.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.8 wiring and pin designations");
  }
  if (LoadCell_9.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.9 wiring and pin designations");
  }
  if (LoadCell_10.getTareTimeoutFlag()) {
    Serial.println("Timeout, check MCU>HX711 no.10 wiring and pin designations");
  }
  LoadCell_1.setCalFactor(calibrationValue_1); // user set calibration value (float)
  LoadCell_2.setCalFactor(calibrationValue_2); // user set calibration value (float)
  LoadCell_3.setCalFactor(calibrationValue_3);
  LoadCell_4.setCalFactor(calibrationValue_4);
  LoadCell_5.setCalFactor(calibrationValue_5);

 
  LoadCell_6.setCalFactor(calibrationValue_6); // user set calibration value (float)
  LoadCell_7.setCalFactor(calibrationValue_7);   // user set calibration value (float)
  LoadCell_8.setCalFactor(calibrationValue_8);
  LoadCell_9.setCalFactor(calibrationValue_9);
  LoadCell_10.setCalFactor(calibrationValue_10);
  
  Serial.println("Startup is complete");
}

void loop() {
  static boolean newDataReady = 0;
  const int serialPrintInterval = 0; //increase value to slow down serial print activity

  // check for new data/start next conversion:
  if (LoadCell_1.update()) newDataReady = true;
  LoadCell_2.update();
  LoadCell_3.update();
  LoadCell_4.update();
  LoadCell_5.update();

  LoadCell_6.update();
  LoadCell_7.update();
  LoadCell_8.update();
  LoadCell_9.update();
  LoadCell_10.update();

  //get smoothed value from data set
  if ((newDataReady)) {
    if (millis() > t + serialPrintInterval) {
      float a = LoadCell_1.getData();
      float b = LoadCell_2.getData();
      float c = LoadCell_3.getData();
      float d = LoadCell_4.getData();
      float e = LoadCell_5.getData();

      float f = LoadCell_6.getData();
      float g = LoadCell_7.getData();
      float h = LoadCell_8.getData();
      float i = LoadCell_9.getData();
      float j = LoadCell_10.getData();

      
    
      Serial.print(a);
      Serial.print(','); 
      Serial.print(b);
      Serial.print(',');
      Serial.print(c);
      Serial.print(',');
      Serial.print(d);
      Serial.print(',');
      Serial.print(e);
      Serial.print(',');
      
      Serial.print(f);
      Serial.print(',');
      Serial.print(g);
      Serial.print(',');
      Serial.print(h);
      Serial.print(',');
      Serial.print(i);
      Serial.print(',');
      Serial.println(j);
      
      newDataReady = 0;
      t = millis();
    }
  }

  // receive command from serial terminal, send 't' to initiate tare operation:
  if (Serial.available() > 0) {
    char inByte = Serial.read();
    if (inByte == 't') {
      LoadCell_1.tareNoDelay();
      LoadCell_2.tareNoDelay();
      LoadCell_3.tareNoDelay();
      LoadCell_4.tareNoDelay();
      LoadCell_5.tareNoDelay();

      LoadCell_6.tareNoDelay();
      LoadCell_7.tareNoDelay();
      LoadCell_8.tareNoDelay();
      LoadCell_9.tareNoDelay();
      LoadCell_10.tareNoDelay();
    }
  }

  //check if last tare operation is complete
  if (LoadCell_1.getTareStatus() == true) {
    Serial.println("Tare load cell 1 complete");
  }
  if (LoadCell_2.getTareStatus() == true) {
    Serial.println("Tare load cell 2 complete");
  }
  if (LoadCell_3.getTareStatus() == true) {
    Serial.println("Tare load cell 3 complete");
  }
  if (LoadCell_4.getTareStatus() == true) {
    Serial.println("Tare load cell 4 complete");
  }
  if (LoadCell_5.getTareStatus() == true) {
    Serial.println("Tare load cell 5 complete");
  }
   if (LoadCell_6.getTareStatus() == true) {
    Serial.println("Tare load cell 6 complete");
  }
  if (LoadCell_7.getTareStatus() == true) {
    Serial.println("Tare load cell 7 complete");
  }
  if (LoadCell_8.getTareStatus() == true) {
    Serial.println("Tare load cell 8 complete");
  }
  if (LoadCell_9.getTareStatus() == true) {
    Serial.println("Tare load cell 9 complete");
  }
  if (LoadCell_10.getTareStatus() == true) {
    Serial.println("Tare load cell 10 complete");
  }

}
