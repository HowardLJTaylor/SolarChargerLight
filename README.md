# SolarChargerLight
Camping Solar Charger Lighting System Arduino


#include "U8glib.h"                             // U8glib library for the OLED
           
U8GLIB_SSD1306_128X64 u8g(U8G_I2C_OPT_NONE|U8G_I2C_OPT_DEV_0);  // I2C / TWI setup OLED

int analogInput = 0;                            //Declare global vsriables
float vin = 0.0;
int value = 0;
int percent = 0;
int timedays = 0;
float ontime = 0;
float ontimeminutes = 0;
float vadjusted = 0;

void draw(void) 
{
  u8g.drawFrame(0, 0, 128, 64);                 // Draws frame with rounded edges
  
  u8g.setFont(u8g_font_profont12);              // Select font
  u8g.drawStr(10, 12, "HTEC SOLAR CHARGER");    // Print Title
 
  u8g.setPrintPos(10,23);
  u8g.println("Battery ");
  u8g.println(vadjusted);                             //Prints the voltage
  u8g.println(" volts");

 
  if (percent>=100) percent = 100;
  u8g.setPrintPos(10,34);                       //Prints percentage
  u8g.println("Cells ");
  u8g.println(percent);
  u8g.println("% charged");


  if (timedays >=0) {
  u8g.setPrintPos(10,46);                       //Prints estimated charge time remaining
  u8g.println(" Full in ");
  u8g.println(timedays);
  u8g.println(" days!");
  } else  {
    u8g.setPrintPos(8,46);
     u8g.println(" -CHARGE COMPLETE-");
  }

  u8g.setPrintPos(10,58);                       //On time data
  u8g.println("Ontime ");
  u8g.println(ontimeminutes);
  u8g.println(" min");

}

void setup(){
   pinMode(analogInput, INPUT);                 //Setup analogue pins
}

void loop(){                                    //Main Loop
   // read the value at analog input
   value = analogRead(analogInput);
   vin = (value *5) / 1024.0;                   // math compenstae voltage
   vadjusted = vin * 0.84;
   percent = value / 11.2;
   timedays = (4.2 - vadjusted) / 0.04;
   ontimeminutes = (ontime / 600);              //Maths for displayed estimations
   ontime=ontime+2.93;
   
  u8g.firstPage();  
  do 
    {
     draw();      
    }
  while( u8g.nextPage() );
delay(50);

}
